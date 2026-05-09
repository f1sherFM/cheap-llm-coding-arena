## TASK
A Django REST Framework service needs a soft delete feature for the Product model. Deleted products should be hidden from the list endpoint by default, but retrievable via a query parameter ?include_deleted=true. The API contract for active products must remain unchanged.

Your task: Implement soft delete. Add the necessary field, filtering logic, and parameter handling. Keep changes minimal. Do not add new dependencies.

## CODEBASE CONTEXT

[FILE: app/models.py]
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=200)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name

[FILE: app/serializers.py]
from rest_framework import serializers
from .models import Product

class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ['id', 'name', 'price', 'created_at']

[FILE: app/views.py]
from rest_framework import viewsets
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer

[FILE: tests/test_products.py]
def test_product_list_returns_active():
    # Existing test assumes all created products are returned
    response = client.get('/products/')
    assert response.status_code == 200
    assert len(response.data) == Product.objects.count()

## CONSTRAINTS
1. Do not change the response schema for active products.
2. Existing tests must continue to pass.
3. Do not add new dependencies.
4. Keep changes minimal and DRF-idiomatic.