# 🔧 Task 04 — Frozen Prompt: Refactor

> **Version:** `v1`  
> **Date:** 2026-05-09  
> **Temperature:** 0.2  
> **Max Tokens:** 4096  
> **Task Type:** Refactor  
> **Language:** Python (async)  
> **Difficulty:** Medium-Hard

---

## System Message

```text
You are a senior software engineer. You will be given a coding task.
Respond with code only. Do not add conversational filler.
Wrap your code in markdown code fences (```python ... ```).
If you need to modify multiple files, output each file separately with its path.
```

---

## Task Description

Refactor the `app/services/legacy_processor.py` module from a callback-based async pattern to modern `async/await` with structured error handling and type hints.

**Current problems:**
- Deeply nested callbacks (`_on_fetch` → `_on_parse` → `_on_save`)
- Errors are silently swallowed or passed as string arguments inside dicts
- Hard to trace control flow
- No type hints

**Your task:** Rewrite the module using `async/await`, explicit exception handling, and type hints. Preserve the exact external behavior: the `process_item` coroutine should accept an `item_id` and return a `dict` with `status` and `result`.

Do not change the calling code in `app/api/items.py`.

---

## Codebase Context

### `app/services/legacy_processor.py`

```python
import asyncio
from app.db import get_db
from app.external.parser import parse_raw

def process_item(item_id, callback):
    db = get_db()

    def _on_fetch(row):
        if row is None:
            callback({"status": "not_found", "result": None})
            return
        raw = row["raw_data"]

        def _on_parse(parsed):
            if parsed.get("error"):
                callback({"status": "parse_error", "result": parsed["error"]})
                return

            def _on_save(success):
                if not success:
                    callback({"status": "save_failed", "result": None})
                else:
                    callback({"status": "ok", "result": parsed["data"]})

            db.execute("UPDATE items SET processed = 1 WHERE id = ?", (item_id,), _on_save)

        parse_raw(raw, _on_parse)

    db.fetchone("SELECT * FROM items WHERE id = ?", (item_id,), _on_fetch)
```

### `app/api/items.py` (caller — must not change)

```python
from fastapi import APIRouter
from app.services.legacy_processor import process_item
import asyncio

router = APIRouter()

@router.post("/items/{item_id}/process")
async def process(item_id: int):
    result = await process_item(item_id)
    return result
```

### `app/external/parser.py`

```python
import asyncio

async def parse_raw(raw: str) -> dict:
    await asyncio.sleep(0.01)
    if not raw.strip():
        return {"error": "empty input"}
    return {"data": raw.upper()}
```

---

## Constraints

1. `process_item` must be an `async def` function.
2. No nested callback functions remain.
3. Exceptions are raised or handled explicitly (not passed as strings inside dicts).
4. The return value contract is preserved: `{"status": "...", "result": ...}`.
5. `app/api/items.py` requires **zero changes** to keep working.
6. No new dependencies.

---

## Output Format

Provide the complete modified file in a markdown code block.

Example:

```python
# app/services/legacy_processor.py
[your code here]
```
