## Q4: Reflective Questions

### 7. What are the main benefits of using JWT for authentication?

- **Stateless**: JWT does not require server-side session storage, making it scalable and suitable for microservices or distributed systems.
- **Compact and portable**: Tokens are small, easy to pass in HTTP headers, and can be used across domains or platforms (e.g., web, mobile).
- **Self-contained**: JWTs carry all the necessary user information, which can be decoded without accessing a database (though validation is still needed).
- **Secure**: When signed and optionally encrypted, JWTs protect against tampering and unauthorized access.

---

### 8. Where should you store your JWT secret and why?

- The JWT secret should be stored in **environment variables** (e.g., `.env` file) and **never hard-coded** in the source code.
- This prevents accidental exposure in version control (like GitHub) and keeps secrets configurable per environment (dev, staging, production).

---

### 9. Why is it important to hash passwords even if the system is protected with JWT?

- **JWT protects access**, but **hashed passwords protect stored credentials**.
- If the database is ever breached, hashed passwords prevent attackers from seeing plain-text passwords.
- Hashing with a strong algorithm like bcrypt ensures passwords are computationally difficult to reverse.

---

### 10. What might happen if a protected route does not check the JWT?

- Any user (even unauthenticated) could access sensitive endpoints.
- This can lead to **data leaks**, **unauthorized actions**, or full **security breaches**.
- Authentication is only useful if every protected route **actually enforces** it.

---

### 11. How does Swagger help frontend developers or API consumers?

- Provides **interactive documentation** that lists available endpoints, request/response formats, and status codes.
- Allows developers to **test APIs directly** in the browser without Postman or manual setups.
- Ensures **clear communication** between backend and frontend teams or third-party consumers.

---

### 12. What tradeoffs come with using token expiration (e.g., 1 hour)?

**Pros:**
- Limits the time a stolen token can be used.
- Encourages re-authentication, reducing long-term access risks.

**Cons:**
- May frustrate users if sessions expire too quickly.
- Requires logic for token renewal (e.g., refresh tokens) to maintain usability.

**Balance:** Short expirations improve security, but should be paired with refresh tokens or automatic renewal to avoid bad UX.
