# ✨ Task 02 — Feature Implementation

> **Type:** Feature Add  
> **Language:** Python (FastAPI)  
> **Difficulty:** Medium  
> **Estimated LOC touched:** 40-80

---

## 📋 Task Description

Add a new `POST /projects` endpoint to the existing FastAPI application. This endpoint should allow authenticated users to create a new project with the following fields:

- `name` (required, string, max 100 chars)
- `description` (optional, string, max 500 chars)
- `owner_id` (required, integer, must match the authenticated user)

The endpoint must:
1. Validate input using Pydantic models.
2. Reject requests where `owner_id` does not match the current authenticated user's ID.
3. Insert the project into the database and return the created record.
4. Return `409 Conflict` if a project with the same `name` already exists for that owner.

**Your task:** Implement the endpoint, validation logic, and any necessary service/serializer changes. Do not modify existing endpoints. Keep changes minimal.

---

## 🧩 Context

### `app/main.py`

```python
from fastapi import FastAPI
from app.api import users, projects

app = FastAPI(title="Task Manager")
app.include_router(users.router, prefix="/api/v1")
app.include_router(projects.router, prefix="/api/v1")
```

### `app/api/users.py`

```python
from fastapi import APIRouter, Depends
from app.auth import get_current_user

router = APIRouter()

@router.get("/users/me")
async def read_users_me(current_user: dict = Depends(get_current_user)):
    return current_user
```

### `app/auth.py`

```python
from fastapi import Header, HTTPException

async def get_current_user(x_user_id: int = Header(...)) -> dict:
    # In production this queries the DB; here we simulate auth.
    if x_user_id <= 0:
        raise HTTPException(status_code=401, detail="Invalid user ID")
    return {"id": x_user_id, "role": "member"}
```

### `app/db.py`

```python
import asyncpg

async def get_db():
    # Simulated dependency; injected via FastAPI Depends
    pass
```

### `app/models/project.py`

```python
from pydantic import BaseModel

class Project(BaseModel):
    id: int
    name: str
    description: str | None
    owner_id: int
    created_at: str
```

### `app/api/projects.py` (new file stub)

```python
from fastapi import APIRouter

router = APIRouter()

# TODO: Implement POST /projects
```

---

## ✅ Success Criteria

1. `POST /api/v1/projects` creates a project when given valid input.
2. Returns `403 Forbidden` (or `401`) if `owner_id` mismatches the authenticated user.
3. Returns `409 Conflict` when the owner already has a project with the same name.
4. Uses existing Pydantic / FastAPI patterns from the codebase.
5. Does not break any existing routes.
6. No new external dependencies.

---

## 🏷️ Scoring Notes

| Category | Focus |
|----------|-------|
| Correctness | Does the endpoint satisfy all functional requirements? |
| Regression Safety | Are existing routes untouched and passing? |
| Context Understanding | Does it reuse existing `get_current_user`, `get_db`, and Pydantic conventions? |
| Code Quality | Is the code idiomatic FastAPI / Python? |
| Tests / Edge Cases | Does it include validation edge cases (empty name, SQL injection patterns, etc.)? |
| Speed / Stability | N/A |
| Manual Fixes Needed | How much cleanup to make it production-ready? |
