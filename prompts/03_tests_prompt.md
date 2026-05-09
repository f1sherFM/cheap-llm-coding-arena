# Frozen Prompt — Task 03 (Tests / Edge Cases)

## System Message
You are a senior software engineer. You will be given a coding task.
Respond with code only. Do not add conversational filler.
Wrap your code in markdown code fences with language tag.
If you need to modify multiple files, output each file separately with its path.

## Task Description
A production Python service uses a discount calculator function that works correctly but has zero test coverage. Business logic is sensitive to pricing errors, so comprehensive test coverage is critical.

Your task: Write a pytest test suite for calculate_discount(user, cart). Cover normal flows, boundary values, overlapping conditions, and invalid inputs. Do not modify the business logic. Keep tests idiomatic and maintainable.

## Codebase Context

[FILE: src/models.py]
from dataclasses import dataclass

@dataclass
class User:
    is_premium: bool
    loyalty_years: int

@dataclass
class CartItem:
    price: float
    qty: int

[FILE: src/discount.py]
def calculate_discount(user: User, cart: list[CartItem]) -> float:
    total = sum(item.price * item.qty for item in cart)
    if total <= 0:
        return 0.0
    
    discount_rate = 0.0
    if user.is_premium:
        discount_rate += 0.10
    if total > 1000:
        discount_rate += 0.05
    if user.loyalty_years >= 3:
        discount_rate += 0.05
    
    # Cap at 20%
    discount_rate = min(discount_rate, 0.20)
    return round(total * discount_rate, 2)

## Constraints
1. Do not modify src/discount.py or src/models.py.
2. Use pytest framework only.
3. Cover edge cases explicitly.
4. Keep tests fast, isolated, and readable.

## Output Format (STRICT)
1. Explanation (max 3 sentences)
2. Test code: provide FULL file content with [FILE: tests/test_discount.py] header
3. Optional: fixtures or helpers in separate block
4. Assumptions (if any)