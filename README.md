# django-auth-jwt-oauth2

Authentication & authorization service built with **Django** and **Django REST Framework**.  
The project demonstrates production-style auth patterns: JWT-based authentication, refresh token rotation, optional 2FA, OAuth2 flows, and request rate limiting using DRF throttling.

---

## Goals

This repository is meant to showcase:

- Implementing **JWT-based authentication** for APIs
- **Refresh token rotation** and secure token revocation
- Basic **authorization** with roles/permissions
- Optional **two-factor authentication (2FA)**
- **Rate limiting** using DRF throttling
- Integrating or acting as an **OAuth2** provider/consumer

It is designed as a portfolio-ready auth service, not a full product.

---

## Tech stack

- **Web framework:** Django
- **API:** Django REST Framework (DRF)
- **Auth:** JWT (e.g. via Simple JWT or custom implementation)
- **OAuth2:** Django OAuth Toolkit / external provider integration
- **Database:** PostgreSQL (or SQLite for local dev)
- **Optional:** Redis/Cache backend for tokens / throttling

---

## Features

### 1. JWT-based authentication

- Issue **access** and **refresh** tokens after a successful login
- Store minimal user info in the JWT payload (e.g. user id, roles/permissions)
- Configurable access token lifetime (short-lived)
- Stateless authentication for API requests:
  - Clients send `Authorization: Bearer <access_token>` header

Endpoints (example):

- `POST /auth/login/` – username/password login → returns access + refresh tokens
- `POST /auth/logout/` – optional blacklist / invalidate refresh tokens
- `POST /auth/token/refresh/` – refresh token → new access/refresh pair

### 2. Refresh token rotation

- **Refresh token rotation** pattern:
  - Every refresh request returns a **new** refresh token
  - The old refresh token is **invalidated** or marked as used
- Protection against token theft:
  - If a stolen refresh token is used again, it can be detected
