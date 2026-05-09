# Task 04 — Refactor: Legacy Auth Middleware

## Meta
Version: v1
Date: 2026-05-09
Temperature: 0.2
Max Tokens: 4096
Task Type: Refactor
Language: Python / Flask
Difficulty: Medium
Bracket: Free / Cheap

## Problem Statement
The auth module works but suffers from duplicated validation logic, magic strings, and missing type annotations. This increases maintenance cost and risk of inconsistencies.

## Goal
Refactor app/auth.py to follow DRY principles, extract validation into a reusable component, add type hints, and preserve exact backward compatibility.

## Files Provided
- app/auth.py — legacy auth logic (read-only for behavior, editable for structure)

## Success Criteria
1. Duplicated token checks removed or centralized
2. Type hints added to all functions and parameters
3. External behavior unchanged (same status codes, same JSON structure)
4. Code is cleaner, more readable, and easier to extend
5. No new dependencies or architectural overhauls

## Scoring (For Judges)
Correctness (0-5): Behavior preserved, no regressions in auth flow
Regression safety (0-5): HTTP responses and status codes match original exactly
Context understanding (0-5): Model identifies duplication, magic values, and missing types
Code quality (0-5): Clean, idiomatic Flask/Python, proper separation of concerns
Tests/edge cases (0-5): Notes or tests confirming backward compatibility (optional but valued)
Speed/stability (0-5): No performance degradation, no heavy abstractions
Manual fixes needed (0-5): Refactored code is production-ready and drop-in replacement

## Expected Solution Pattern
- Extract token validation into a dedicated function or decorator
- Remove duplicated if/return blocks from routes
- Add type hints (str, Optional[str], Tuple[Response, int], etc.)
- Keep SECRET as module-level constant or move to config (acceptable)
- Preserve exact error messages and status codes

## Notes for Judges
- Accept decorator approach or helper function + wrapper
- Reject solutions that change response format or status codes
- Reject over-engineering (e.g., full JWT library integration, database auth)
- Bonus: clear separation of validation vs routing, descriptive function names, PEP-8 compliance