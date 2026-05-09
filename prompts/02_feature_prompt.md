# ✨ Task 02 — Frozen Prompt: Feature Implementation

> **Version:** `v1`  
> **Date:** 2026-05-09  
> **Temperature:** 0.2  
> **Max Tokens:** 4096  
> **Task Type:** Feature Add  
> **Language:** Python (FastAPI)  
> **Difficulty:** Medium

---

## System Message

```text
You are a senior software engineer. You will be given a coding task.
Respond with code only. Do not add conversational filler.
Wrap your code in markdown code fences (```python ... ```).
If you need to create or modify multiple files, output each file separately with its path.
```

---

## Task Description

Add a new `POST /projects` endpoint to the existing FastAPI application. This endpoint should allow authenticated users to create a new project with the following fields:

- `name` (required, string, max 100 chars)
- `description` (optional, string, max 500 chars)
- `owner_id` (required, integer, must match the authenticated user)

The endpoint must:
1. Validate input using Pydantic models.
2. Reject requests where `owner_id` does not match the current authenticated user's ID.
3. Insert the project into the database and return the created record.
4. Return `409 Conflict` if a project with the same `name` already exists for that owner.

**Your task:** Implement the endpoint, validation logic, and any necessary service/serializer changes. Do not modify existing endpoints. Keep changes minimal. No new dependencies.

---

## Codebase Context

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
    if x_user_id <= 0:
        raise HTTPException(status_code=401, detail="Invalid user ID")
    return {"id": x_user_id, "role": "member"}
```

### `app/db.py`

```python
import asyncpg

async def get_db():
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
```

---

## Constraints

1. Do not modify existing endpoints in `users.py` or `main.py`.
2. Use existing patterns (`get_current_user`, `get_db`, Pydantic) where possible.
3. No new external dependencies.
4. Keep changes minimal.

---

## Output Format

Provide the complete modified or new file(s) in markdown code blocks with the file path as a comment at the top.

Example:

```python
# app/api/projects.py
[your code here]
```
