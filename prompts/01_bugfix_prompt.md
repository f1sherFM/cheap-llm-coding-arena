# Frozen Prompt — Task 01 (Bug Fix)

## System Message
You are a senior software engineer. You will be given a coding task.
Respond with code only. Do not add conversational filler.
Wrap your code in markdown code fences with language tag.
If you need to modify multiple files, output each file separately with its path.

## Task Description
A production FastAPI service is experiencing intermittent 500 Internal Server Error responses on the /users/{user_id} endpoint. Logs show a KeyError originating from the user serializer when the profile field is missing from the database response.

Your task: Fix the bug. Do not change the API contract or the database schema. Keep changes minimal. Do not add new dependencies.

## Codebase Context

[FILE: app/api/users.py]
from fastapi import APIRouter, Depends, HTTPException
from app.services.user_service import get_user_by_id
from app.serializers.user_serializer import serialize_user

router = APIRouter()

@router.get("/users/{user_id}")
async def get_user(user_id: int, db=Depends(get_db)):
    raw_user = await get_user_by_id(db, user_id)
    if not raw_user:
        raise HTTPException(status_code=404, detail="User not found")
    return serialize_user(raw_user)

[FILE: app/serializers/user_serializer.py]
def serialize_user(raw):
    return {
        "id": raw["id"],
        "email": raw["email"],
        "profile": {
            "display_name": raw["profile"]["display_name"],
            "avatar_url": raw["profile"]["avatar_url"],
            "bio": raw["profile"]["bio"],
        },
        "created_at": raw["created_at"].isoformat(),
    }

[FILE: app/services/user_service.py]
async def get_user_by_id(db, user_id: int):
    row = await db.fetchrow("SELECT * FROM users WHERE id = $1", user_id)
    if not row:
        return None
    return dict(row)

[FILE: tests/test_users.py]
def test_serialize_user_complete():
    raw = {
        "id": 1,
        "email": "alice@example.com",
        "profile": {
            "display_name": "Alice",
            "avatar_url": "https://cdn.example.com/a.png",
            "bio": "Hello",
        },
        "created_at": datetime(2024, 1, 1, tzinfo=timezone.utc),
    }
    result = serialize_user(raw)
    assert result["profile"]["display_name"] == "Alice"

## Constraints
1. Do not change the API response schema for users who do have a profile.
2. Existing tests must continue to pass.
3. Do not add new dependencies.
4. Keep changes minimal.

## Output Format (STRICT)
1. Explanation (max 3 sentences)
2. Code changes: provide FULL file content with [FILE: path] header
3. Optional: new/updated tests in separate block
4. Assumptions (if any)