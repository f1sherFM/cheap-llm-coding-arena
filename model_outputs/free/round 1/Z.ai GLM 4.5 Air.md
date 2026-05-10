## Explanation
The bug occurs in the user serializer when the profile field is missing from the database response. The code directly accesses nested profile fields without checking if the profile exists, causing a KeyError. The fix adds a conditional check to handle missing profile fields gracefully.

## Code changes

[FILE: app/serializers/user_serializer.py]
```python
def serialize_user(raw):
    return {
        "id": raw["id"],
        "email": raw["email"],
        "profile": {
            "display_name": raw["profile"]["display_name"],
            "avatar_url": raw["profile"]["avatar_url"],
            "bio": raw["profile"]["bio"],
        } if raw["profile"] is not None else None,
        "created_at": raw["created_at"].isoformat(),
    }
```

## Optional: new/updated tests
```python
def test_serialize_user_missing_profile():
    raw = {
        "id": 1,
        "email": "alice@example.com",
        "profile": None,
        "created_at": datetime(2024, 1, 1, tzinfo=timezone.utc),
    }
    result = serialize_user(raw)
    assert result["profile"] is None
```

## Assumptions
- The database returns `None` for missing profile fields, which is then converted to a dictionary with a `None` value for the "profile" key.
- The API should return `null` for the profile field when it doesn't exist in the database.

---------------------------------------------

```markdown
## Explanation
I'll implement soft delete by adding a deleted timestamp field to the Product model, modifying the viewset to filter out deleted items by default, and adding query parameter support to include deleted items when requested.

[FILE: app/models.py]
from django.db import models
from django.utils import timezone

class Product(models.Model):
    name = models.CharField(max_length=200)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)
    deleted_at = models.DateTimeField(null=True, blank=True)

    def __str__(self):
        return self.name

    def soft_delete(self):
        self.deleted_at = timezone.now()
        self.save()

[FILE: app/serializers.py]
from rest_framework import serializers
from .models import Product

class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ['id', 'name', 'price', 'created_at', 'deleted_at']

[FILE: app/views.py]
from rest_framework import viewsets
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    serializer_class = ProductSerializer
    
    def get_queryset(self):
        queryset = Product.objects.all()
        include_deleted = self.request.query_params.get('include_deleted', 'false')
        
        if include_deleted.lower() != 'true':
            queryset = queryset.filter(deleted_at__isnull=True)
        
        return queryset

[FILE: tests/test_products.py]
from django.test import TestCase
from django.urls import reverse
from rest_framework.test import APIClient
from rest_framework import status
from .models import Product
import datetime
from django.utils import timezone

def test_product_list_returns_active():
    # Create test products
    active_product = Product.objects.create(name="Active Product", price=19.99)
    deleted_product = Product.objects.create(name="Deleted Product", price=29.99)
    deleted_product.soft_delete()
    
    # Test without include_deleted parameter
    client = APIClient()
    response = client.get('/products/')
    assert response.status_code == 200
    assert len(response.data) == 1
    assert response.data[0]['id'] == active_product.id
    
    # Test with include_deleted=true
    response = client.get('/products/?include_deleted=true')
    assert response.status_code == 200
    assert len(response.data) == 2
```

Assumptions:
1. The Product model doesn't currently have a deleted_at field
2. The API endpoint for products is already set up and working
3. The soft delete functionality should be implemented without affecting any other models or functionality