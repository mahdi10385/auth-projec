# API Documentation

Base URL: `http://localhost:8000/api`

## Required headers

| Header | Value | When |
|--------|-------|------|
| Accept | application/json | Always |
| Content-Type | application/json | When sending a body |
| Authorization | Bearer {token} | Protected routes |

## Endpoints

| Method | URL | Auth | Description |
|--------|-----|------|-------------|
| POST | /register | No | Create a new account |
| POST | /login | No | Log in (max 5 attempts per minute) |
| GET | /me | Yes | Get the current user |
| POST | /logout | Yes | Revoke the current token |

---

## POST /register

Request body:

```json
{
  "name": "Ali",
  "email": "ali@example.com",
  "password": "12345678",
  "password_confirmation": "12345678"
}
```

Rules: `name` required, max 255. `email` required, valid, unique. `password` required, min 8, must match `password_confirmation`.

Success `201`:

```json
{
  "user": {
    "id": 2,
    "name": "Ali",
    "email": "ali@example.com",
    "created_at": "2026-10-08T10:00:00.000000Z",
    "updated_at": "2026-10-08T10:00:00.000000Z"
  },
  "token": "1|abcdef..."
}
```

## POST /login

Request body:

```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

Success `200`: same shape as register (`user` and `token`).

Wrong credentials `401`:

```json
{
  "message": "Invalid email or password."
}
```

## GET /me

Header: `Authorization: Bearer {token}`

Success `200`:

```json
{
  "user": {
    "id": 1,
    "name": "Test User",
    "email": "test@example.com"
  }
}
```

## POST /logout

Header: `Authorization: Bearer {token}`

Success `200`:

```json
{
  "message": "Logged out successfully."
}
```

---

## Error responses

Validation error `422`:

```json
{
  "message": "The email has already been taken.",
  "errors": {
    "email": ["The email has already been taken."]
  }
}
```

Missing or invalid token `401`:

```json
{
  "message": "Unauthenticated."
}
```

Too many requests `429`: login was attempted more than 5 times in a minute. Wait and retry.

## Frontend notes

- Store the `token` after login or register (for example in `localStorage`).
- Send it in the `Authorization` header on protected routes.
- On logout, call `POST /logout` and then delete the stored token.
- If any protected route returns `401`, delete the token and redirect to the login page.