# 🧪 Task 03 — Test Writing

> **Type:** Test Generation  
> **Language:** Python  
> **Difficulty:** Medium  
> **Estimated LOC touched:** 30-60

---

## 📋 Task Description

Write comprehensive unit tests for the `app/utils/pagination.py` module. This module provides a helper that slices a list into paginated chunks with metadata.

**Your task:** Produce a `tests/test_pagination.py` file with thorough tests. Cover:

- Normal usage
- Empty input
- Single-page results
- Out-of-range page numbers
- Invalid inputs (negative page size, zero page size)
- Large page sizes relative to data length

Do not modify the source module. Add your tests only.

---

## 🧩 Context

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

## ✅ Success Criteria

1. All tests are valid `pytest` test functions.
2. At least 6 distinct test cases covering the categories listed above.
3. Tests use `assert` statements with clear intent.
4. No modifications to `app/utils/pagination.py`.
5. Tests run successfully with `pytest tests/test_pagination.py`.

---

## 🏷️ Scoring Notes

| Category | Focus |
|----------|-------|
| Correctness | Do the tests accurately verify the spec? |
| Regression Safety | N/A (no existing code to break) |
| Context Understanding | Do tests respect the existing function signature and docstring contract? |
| Code Quality | Are tests readable, well-named, and DRY (e.g., using `pytest.mark.parametrize`)? |
| Tests / Edge Cases | Are edge cases comprehensive? |
| Speed / Stability | N/A |
| Manual Fixes Needed | How many syntax / import fixes are needed? |
