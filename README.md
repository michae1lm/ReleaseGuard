# ReleaseGuard — Authentication Prototype

A standalone C++ authentication prototype created during a WSU software engineering group project. This repository includes the prototype's source, test harness, and sprint-planning notes.

> **Scope note:** The authentication component is an **independently testable prototype**. Its presence here does not establish that it was merged into the team application or released to users.

## What's in this repository

- `AuthService.h` / `AuthService.cpp` — registration, login, username/password validation, and local account persistence.
- `auth_tests.cpp` — assertion-based tests for registration, duplicate rejection, invalid input, login, persistence, and avoidance of plaintext passwords.
- `auth_demo.cpp` — standalone demonstration.
- `auth_issues.md` and sprint notes — proposed acceptance criteria and planning context.

## Security design

- Passwords are processed with PBKDF2-HMAC-SHA256 (200,000 iterations) using a per-account random salt generated through OpenSSL.
- Saved records contain username, salt, and derived digest—not plaintext passwords.
- Login compares fixed-length digest strings without returning which credential is incorrect.
- Username and minimum password-length checks reduce invalid input, but this is **not** a production identity-management service.

## Build and test (Linux / WSL)

Requirements: C++17 compiler and OpenSSL development headers/library (`libssl-dev` on Ubuntu).

```bash
g++ -std=c++17 -Wall -Wextra AuthService.cpp auth_tests.cpp -lcrypto -o auth_tests
./auth_tests
g++ -std=c++17 -Wall -Wextra AuthService.cpp auth_demo.cpp -lcrypto -o auth_demo
./auth_demo
```

The tests write and remove a temporary local account file. Do not supply real account credentials.

## Architecture

```text
Registration / Login -> AuthService -> OpenSSL PBKDF2 password derivation
                               |
                               +-- local username:salt:digest file
```

## Limitations and next steps

- Standalone prototype only; integration with the team application requires confirmation.
- Local file persistence is educational, not a production credential store. It lacks file locking, access control hardening, account recovery, account lockout/rate limiting, and secure provisioning.
- Review build commands and tests on your own machine before claiming verified execution.
- Group contributions and original repository ownership should be attributed separately from this independent prototype.

**Portfolio takeaway:** The code illustrates secure password storage fundamentals, C++ development, and test-first authentication behavior—not a deployed production service.
