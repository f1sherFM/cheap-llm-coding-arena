# Task 01 — Bug Fix: KeyError in User Serializer

## Meta
Version: v1
Date: 2026-05-09
Temperature: 0.2
Max Tokens: 4096
Task Type: Bug Fix
Language: Python
Difficulty: Medium
Bracket: Free / Cheap

## Problem Statement
A production FastAPI service returns 500 Internal Server Error on GET /users/{user_id} when a user has no linked profile. The error is KeyError: 'profile' in the serializer.

## Goal
Fix the serializer to handle missing profile gracefully without changing the API contract or breaking existing tests.

## Files Provided
- app/api/users.py — endpoint handler
- app/serializers/user_serializer.py — contains the bug
- app/services/user_service.py — data fetching layer
- tests/test_users.py — existing test (must pass)

## Success Criteria
1. No KeyError when raw["profile"] is missing or None
2. Response schema unchanged for users WITH profile
3. Existing test test_serialize_user_complete still passes
4. Minimal changes — no refactoring, no new dependencies

## Scoring (For Judges)
Correctness (0-5): Bug fixed, no new errors
Regression safety (0-5): Existing tests pass
Context understanding (0-5): Model understood optional profile
Code quality (0-5): Clean, readable, minimal
Tests/edge cases (0-5): Added test for profile is None
Speed/stability (0-5): No heavy ops or new deps
Manual fixes needed (0-5): Patch applies cleanly

## Expected Solution Pattern
Use .get() or conditional check for safe access:
profile = raw.get("profile") or {}
then access profile.get("field") instead of raw["profile"]["field"]

## Notes for Judges
- Accept both .get() and if profile: patterns
- Reject solutions that change response schema for happy path
- Reject solutions that add try/except without explanation
- Bonus: model adds a test case for missing profile