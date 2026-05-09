# Task 02 — Feature: Soft Delete with Query Filter

## Meta
Version: v1
Date: 2026-05-09
Temperature: 0.2
Max Tokens: 4096
Task Type: Feature Implementation
Language: Python / Django REST Framework
Difficulty: Medium
Bracket: Free / Cheap

## Problem Statement
The product catalog requires a soft delete mechanism. Deleted items must be excluded from standard list views but accessible for admin/recovery purposes via a query parameter.

## Goal
Add a deleted_at field to the Product model. Modify the view to filter out deleted products by default. Add support for ?include_deleted=true to override the filter.

## Files Provided
- app/models.py — Product model
- app/serializers.py — ProductSerializer
- app/views.py — ProductViewSet
- tests/test_products.py — existing test (must pass)

## Success Criteria
1. deleted_at field added to model (nullable)
2. List endpoint hides deleted products by default
3. ?include_deleted=true returns all products
4. Existing tests pass without modification
5. Minimal changes, follows DRF conventions

## Scoring (For Judges)
Correctness (0-5): Soft delete implemented, filtering works correctly
Regression safety (0-5): Existing tests pass, active product schema unchanged
Context understanding (0-5): Model understands DRF patterns (get_queryset override)
Code quality (0-5): Clean, idiomatic DRF, no over-engineering
Tests/edge cases (0-5): Adds test for include_deleted param and deleted state
Speed/stability (0-5): Efficient queryset filtering, no N+1 or heavy ops
Manual fixes needed (0-5): Changes apply cleanly, standard DRF structure

## Expected Solution Pattern
1. Add deleted_at = models.DateTimeField(null=True, blank=True) to model
2. Override get_queryset() in ViewSet:
   qs = Product.objects.all()
   if not self.request.query_params.get('include_deleted'):
       qs = qs.filter(deleted_at__isnull=True)
   return qs
3. Keep serializer unchanged

## Notes for Judges
- Accept get_queryset override or custom filter backend
- Reject solutions that delete records permanently
- Reject solutions that change default list behavior for active items
- Bonus: proper migration note or handles soft delete in create/update logic