Explanation
The serialize_user function crashes with a KeyError when raw["profile"] is None or missing. The fix uses .get("profile", {}) to safely handle missing profiles, and uses .get() for each profile field so that None values are returned instead of raising errors. The API contract is preserved — users with a profile still get the full nested object, and users without one get None values for each profile field.

[FILE: app/serializers/user_serializer.py]
def serialize_user(raw):
    profile = raw.get("profile") or {}
    return {
        "id": raw["id"],
        "email": raw["email"],
        "profile": {
            "display_name": profile.get("display_name"),
            "avatar_url": profile.get("avatar_url"),
            "bio": profile.get("bio"),
        },
        "created_at": raw["created_at"].isoformat(),
    }

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

def test_serialize_user_missing_profile():
    raw = {
        "id": 2,
        "email": "bob@example.com",
        "profile": None,
        "created_at": datetime(2024, 1, 1, tzinfo=timezone.utc),
    }
    result = serialize_user(raw)
    assert result["id"] == 2
    assert result["email"] == "bob@example.com"
    assert result["profile"]["display_name"] is None
    assert result["profile"]["avatar_url"] is None
    assert result["profile"]["bio"] is None


Assumptions:

When profile is missing or None, the profile fields should be None rather than omitting the profile key entirely, keeping the response schema consistent.
The created_at field is always present and is a datetime object (no change needed there).

-------------------------------------------------------------------

Soft delete is implemented by adding an `is_deleted` field (default=False) to the Product model and filtering the queryset in the viewset. The `get_queryset` method checks for `?include_deleted=true` to optionally include deleted products. This maintains backward compatibility and follows DRF conventions.

[FILE: app/models.py]
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=200)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)
    is_deleted = models.BooleanField(default=False)

    def __str__(self):
        return self.name

[FILE: app/views.py]
from rest_framework import viewsets
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    serializer_class = ProductSerializer

    def get_queryset(self):
        queryset = Product.objects.all()
        if self.request.query_params.get('include_deleted') != 'true':
            queryset = queryset.filter(is_deleted=False)
        return queryset

Assumptions: Existing products have `is_deleted=False` by default. The test uses `Product.objects.count()` which now respects the filtered queryset since the view no longer uses `queryset = Product.objects.all()` statically.

--------------------------------------------------------------

## Explanation

The `calculate_discount` function has clear business rules: premium users get 10%, carts over $1000 get 5%, and loyalty ≥3 years adds 5%, capped at 20%. I'll test all combinations, boundary values, and edge cases without modifying the source code.

[FILE: tests/test_discount.py]
```python
import pytest
from src.models import User, CartItem
from src.discount import calculate_discount


class TestCalculateDiscount:
    """Test suite for calculate_discount function."""

    # Helper methods
    def _make_user(self, is_premium=False, loyalty_years=0):
        return User(is_premium=is_premium, loyalty_years=loyalty_years)

    def _make_cart(self, items):
        """items: list of (price, qty) tuples"""
        return [CartItem(price=p, qty=q) for p, q in items]

    # Normal flows
    def test_no_discount_regular_user_small_cart(self):
        user = self._make_user(is_premium=False, loyalty_years=0)
        cart = self._make_cart([(100.0, 1)])
        assert calculate_discount(user, cart) == 0.0

    def test_premium_user_only(self):
        user = self._make_user(is_premium=True, loyalty_years=0)
        cart = self._make_cart([(100.0, 1)])
        assert calculate_discount(user, cart) == 10.0  # 10% of 100

    def test_high_value_cart_only(self):
        user = self._make_user(is_premium=False, loyalty_years=0)
        cart = self._make_cart([(1001.0, 1)])
        assert calculate_discount(user, cart) == 50.05  # 5% of 1001

    def test_loyal_user_only(self):
        user = self._make_user(is_premium=False, loyalty_years=3)
        cart = self._make_cart([(100.0, 1)])
        assert calculate_discount(user, cart) == 5.0  # 5% of 100

    def test_premium_and_high_value(self):
        user = self._make_user(is_premium=True, loyalty_years=0)
        cart = self._make_cart([(1001.0, 1)])
        assert calculate_discount(user, cart) == 150.15  # 15% of 1001

    def test_all_discounts_combined(self):
        user = self._make_user(is_premium=True, loyalty_years=5)
        cart = self._make_cart([(2000.0, 1)])
        assert calculate_discount(user, cart) == 400.0  # 20% of 2000 (capped)

    # Boundary values
    def test_cart_total_exactly_1000_no_high_value_discount(self):
        user = self._make_user(is_premium=False, loyalty_years=0)
        cart = self._make_cart([(1000.0, 1)])
        assert calculate_discount(user, cart) == 0.0

    def test_cart_total_just_over_1000(self):
        user = self._make_user(is_premium=False, loyalty_years=0)
        cart = self._make_cart([(1000.01, 1)])
        expected = round(1000.01 * 0.05, 2)
        assert calculate_discount(user, cart) == expected

    def test_loyalty_exactly_3_years(self):
        user = self._make_user(is_premium=False, loyalty_years=3)
        cart = self._make_cart([(100.0, 1)])
        assert calculate_discount(user, cart) == 5.0

    def test_loyalty_2_years_no_discount(self):
        user = self._make_user(is_premium=False, loyalty_years=2)
        cart = self._make_cart([(100.0, 1)])
        assert calculate_discount(user, cart) == 0.0

    # Edge cases
    def test_empty_cart(self):
        user = self._make_user(is_premium=True, loyalty_years=5)
        cart = []
        assert calculate_discount(user, cart) == 0.0

    def test_zero_total_cart(self):
        user = self._make_user(is_premium=True, loyalty_years=5)
        cart = self._make_cart([(0.0, 5)])
        assert calculate_discount(user, cart) == 0.0

    def test_negative_total_cart(self):
        user = self._make_user(is_premium=True, loyalty_years=5)
        cart = self._make_cart([(-100.0, 1)])
        assert calculate_discount(user, cart) == 0.0

    def test_multiple_items_in_cart(self):
        user = self._make_user(is_premium=True, loyalty_years=0)
        cart = self._make_cart([(100.0, 2), (50.0, 3)])  # Total = 350
        assert calculate_discount(user, cart) == 35.0  # 10% of 350

    def test_discount_cap_at_20_percent(self):
        user = self._make_user(is_premium=True, loyalty_years=10)
        cart = self._make_cart([(5000.0, 1)])
        # Would be 25% (10+5+5+5) but capped at 20%
        assert calculate_discount(user, cart) == 1000.0  # 20% of 5000

    def test_rounding_to_two_decimals(self):
        user = self._make_user(is_premium=True, loyalty_years=0)
        cart = self._make_cart([(33.33, 1)])  # 10% = 3.333 -> 3.33
        assert calculate_discount(user, cart) == 3.33

    def test_large_cart_with_all_discounts(self):
        user = self._make_user(is_premium=True, loyalty_years=5)
        cart = self._make_cart([(100.0, 20)])  # Total = 2000
        assert calculate_discount(user, cart) == 400.0  # 20% of 2000

    def test_single_item_quantity(self):
        user = self._make_user(is_premium=False, loyalty_years=0)
        cart = self._make_cart([(100.0, 1)])
        assert calculate_discount(user, cart) == 0.0

    def test_fractional_prices(self):
        user = self._make_user(is_premium=True, loyalty_years=0)
        cart = self._make_cart([(99.99, 1)])
        expected = round(99.99 * 0.10, 2)
        assert calculate_discount(user, cart) == expected
```

## Assumptions
- The discount cap is exactly 20% (not more)
- Negative cart totals should return 0 discount (not raise an error)
- Rounding uses Python's built-in `round()` function
- The function handles empty carts gracefully

----------------------------------------------------------------

1. Explanation: The auth module has duplicated token validation logic in both route functions, magic values scattered throughout, and no type hints. I'll extract a decorator for token validation, add constants for magic values, and add type hints while preserving exact behavior and response formats.

2. Code changes:

[FILE: app/auth.py]
import functools
from flask import request, jsonify, Response, Tuple
from typing import Callable, Any

SECRET = "hardcoded_secret_123"
MIN_TOKEN_LENGTH = 10
TOKEN_PREFIX_BLACKLIST = "bad_"

def check_token(token: str) -> bool:
    if not token:
        return False
    if len(token) < MIN_TOKEN_LENGTH:
        return False
    if token.startswith(TOKEN_PREFIX_BLACKLIST):
        return False
    return True

def validate_token() -> Tuple[Response, int] | None:
    token = request.headers.get("Authorization")
    if not token:
        return jsonify({"error": "Missing token"}), 401
    if not check_token(token):
        return jsonify({"error": "Invalid token"}), 401
    if len(token) < MIN_TOKEN_LENGTH:
        return jsonify({"error": "Token too short"}), 400
    return None

def protected_route(func: Callable[..., Any]) -> Callable[..., Any]:
    @functools.wraps(func)
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        error_response = validate_token()
        if error_response is not None:
            return error_response
        return func(*args, **kwargs)
    return wrapper

@protected_route
def protected_route1() -> Response:
    return jsonify({"data": "secret1"})

@protected_route
def protected_route2() -> Response:
    return jsonify({"data": "secret2"})
```

3. Optional: new/updated tests in separate block

```python
# tests/test_auth.py
import pytest
from app.auth import check_token, validate_token, protected_route1, protected_route2

class TestCheckToken:
    def test_empty_token_returns_false(self):
        assert check_token("") is False
    
    def test_none_token_returns_false(self):
        assert check_token(None) is False
    
    def test_short_token_returns_false(self):
        assert check_token("short") is False
    
    def test_blacklisted_prefix_returns_false(self):
        assert check_token("bad_token_123") is False
    
    def test_valid_token_returns_true(self):
        assert check_token("valid_token_123") is True
    
    def test_exactly_min_length_token_returns_true(self):
        assert check_token("a" * 10) is True
    
    def test_one_below_min_length_returns_false(self):
        assert check_token("a" * 9) is False

class TestProtectedRoutes:
    def test_missing_token_returns_401(self, client):
        response = client.get('/route1')
        assert response.status_code == 401
        assert response.json["error"] == "Missing token"
    
    def test_invalid_token_returns_401(self, client):
        response = client.get('/route1', headers={"Authorization": "bad_token"})
        assert response.status_code == 401
        assert response.json["error"] == "Invalid token"
    
    def test_short_token_returns_400(self, client):
        response = client.get('/route1', headers={"Authorization": "short"})
        assert response.status_code == 400
        assert response.json["error"] == "Token too short"
    
    def test_valid_token_returns_success(self, client):
        response = client.get('/route1', headers={"Authorization": "valid_token_123"})
        assert response.status_code == 200
        assert response.json["data"] == "secret1"
```

4. Assumptions: The Flask app has a test client available. The decorator preserves the original function signatures and behavior exactly. The validation logic order is preserved (missing → invalid → short) to maintain backward compatibility.
