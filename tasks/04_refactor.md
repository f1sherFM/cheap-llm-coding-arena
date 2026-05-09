## TASK
A production Flask service contains a legacy auth module with duplicated validation logic, magic values, and poor readability. The code works but is hard to maintain and extend.

Your task: Refactor the auth module to improve readability, remove duplication, and add type hints. Preserve 100% backward compatibility. Do not change external behavior or HTTP response formats. Keep changes minimal and idiomatic.

## CODEBASE CONTEXT

[FILE: app/auth.py]
import functools
from flask import request, jsonify

SECRET = "hardcoded_secret_123"

def check_token(token):
    if not token:
        return False
    if len(token) < 10:
        return False
    if token.startswith("bad_"):
        return False
    return True

def protected_route1():
    token = request.headers.get("Authorization")
    if not token:
        return jsonify({"error": "Missing token"}), 401
    if not check_token(token):
        return jsonify({"error": "Invalid token"}), 401
    if len(token) < 10:
        return jsonify({"error": "Token too short"}), 400
    return jsonify({"data": "secret1"})

def protected_route2():
    token = request.headers.get("Authorization")
    if not token:
        return jsonify({"error": "Missing token"}), 401
    if not check_token(token):
        return jsonify({"error": "Invalid token"}), 401
    if len(token) < 10:
        return jsonify({"error": "Token too short"}), 400
    return jsonify({"data": "secret2"})

## CONSTRAINTS
1. Do not change the external behavior or response schemas.
2. Do not add new dependencies or change HTTP status codes.
3. Remove duplicated validation logic.
4. Add type hints to functions and parameters.
5. Keep refactoring minimal and focused on auth.py only.