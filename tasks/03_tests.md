# Task 03 — Tests: Edge Cases for Discount Calculator

## Meta
Version: v1
Date: 2026-05-09
Temperature: 0.2
Max Tokens: 4096
Task Type: Tests / Edge Cases
Language: Python
Difficulty: Medium
Bracket: Free / Cheap

## Problem Statement
The discount calculation logic is production-critical but lacks automated tests. Manual verification is error-prone and slows down releases.

## Goal
Create a comprehensive pytest suite that validates correct behavior across normal cases, boundary conditions, overlapping discount rules, and invalid inputs.

## Files Provided
- src/models.py — User and CartItem dataclasses
- src/discount.py — calculate_discount function (read-only)

## Success Criteria
1. Tests run cleanly with pytest (no syntax or import errors)
2. Covers: empty cart, zero/negative prices, premium threshold, loyalty threshold, >1000 total, discount cap (20%), overlapping rules
3. Uses pytest best practices (fixtures, parametrization, clear assertions)
4. Does not modify business logic or add dependencies

## Scoring (For Judges)
Correctness (0-5): Assertions match expected behavior, no false positives/negatives
Regression safety (0-5): Tests would catch real bugs if logic changes
Context understanding (0-5): Model identifies all relevant edge cases and boundaries
Code quality (0-5): Clean pytest structure, parametrization where appropriate, readable
Tests/edge cases (0-5): Comprehensive coverage including cap, overlap, invalid inputs
Speed/stability (0-5): Fast execution, no heavy setup, isolated tests
Manual fixes needed (0-5): Tests run out-of-the-box with standard pytest

## Expected Solution Pattern
- Use pytest fixtures for base User and CartItem objects
- Parametrize tests for multiple input combinations
- Test boundaries: total=0, total=1000, total=1000.01
- Test cap: verify discount never exceeds 20%
- Test invalid/edge: empty cart, negative price, zero qty
- Clear assertion messages

## Notes for Judges
- Accept both class-based and function-based test styles
- Reject tests that mock the function under test
- Reject tests with unclear or missing assertions
- Bonus: parametrized tables, descriptive test names, coverage of float precision edge cases