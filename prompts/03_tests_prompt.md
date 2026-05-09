# 🧪 Task 03 — Frozen Prompt: Test Writing

> **Version:** `v1`  
> **Date:** 2026-05-09  
> **Temperature:** 0.2  
> **Max Tokens:** 4096  
> **Task Type:** Test Generation  
> **Language:** Python  
> **Difficulty:** Medium

---

## System Message

```text
You are a senior software engineer. You will be given a coding task.
Respond with code only. Do not add conversational filler.
Wrap your code in markdown code fences (```python ... ```).
Output the complete file content.
```

---

## Task Description

Write comprehensive unit tests for the `app/utils/pagination.py` module. This module provides a helper that slices a list into paginated chunks with metadata.

Cover the following scenarios:

- Normal usage (multiple pages)
- Empty input list
- Single-page results (data fits in one page)
- Out-of-range page numbers
- Invalid inputs (negative page size, zero page size)
- Large page sizes relative to data length

**Your task:** Produce a complete `tests/test_pagination.py` file. Do not modify the source module. Add tests only.

---

## Codebase Context

### `app/utils/pagination.py`

```python
def paginate(data, page, page_size):
    """
    Slice a list into a paginated response.

    Args:
        data: List of items.
        page: 1-indexed page number.
        page_size: Number of items per page.

    Returns:
        dict: {
            "items": [...],
            "page": page,
            "page_size": page_size,
            "total": len(data),
            "pages": total_pages,
        }
    """
    if page_size <= 0:
        raise ValueError("page_size must be positive")
    if page < 1:
        raise ValueError("page must be >= 1")

    total = len(data)
    pages = (total + page_size - 1) // page_size
    start = (page - 1) * page_size
    end = start + page_size

    return {
        "items": data[start:end],
        "page": page,
        "page_size": page_size,
        "total": total,
        "pages": pages,
    }
```

---

## Constraints

1. All tests must be valid `pytest` test functions.
2. At least 6 distinct test cases.
3. Use `assert` statements with clear intent.
4. No modifications to `app/utils/pagination.py`.
5. Tests must run successfully with `pytest tests/test_pagination.py`.

---

## Output Format

Provide the complete test file in a single markdown code block.

Example:

```python
# tests/test_pagination.py
[your code here]
```
