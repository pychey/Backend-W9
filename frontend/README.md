# Authentication System Overview

## 1. Why do we use localStorage to store the JWT token instead of saving it in a React state? What are the advantages and risks?

### Advantages:
- **Persistence:** `localStorage` persists across browser sessions, so users remain logged in even after closing the browser.
- **Shared across tabs:** The token is available in all tabs/windows of the same origin.
- **Automatic availability:** The token is immediately available on page refresh without requiring re-authentication.

### Risks:
- **XSS vulnerability:** Malicious scripts can access `localStorage` and steal tokens.
- **No automatic expiration:** Tokens remain until manually removed.
- **Storage limitations:** `localStorage` has size limits (typically 5-10MB).

### Alternative approaches:
- Using `httpOnly` cookies with secure flags provides better security but requires server-side session management.

---

## 2. How does the AuthContext improve the way we manage user authentication across different pages?

### Benefits:
- **Global state management:** Authentication state is accessible from any component without prop drilling.
- **Centralized logic:** All auth-related functions (login, logout, token validation) are in one place.
- **Reactive updates:** Components automatically re-render when auth state changes.
- **Consistent behavior:** Ensures all parts of the app have the same understanding of authentication status.
- **Easy testing:** Auth logic can be tested independently and mocked easily.

---

## 3. What would happen if the token in localStorage is expired or tampered with? How should our app handle such a case?

### What happens:
- **Expired token:** JWT payload contains expiration time; our `decodeToken` function checks this.
- **Tampered token:** Base64 decoding will fail or signature verification on the server will fail.

### How we handle it:
- **Client-side:** Check expiration in `decodeToken` and remove invalid tokens.
- **Server-side:** API returns `401 Unauthorized` for invalid tokens.
- **Interceptor:** Axios interceptor catches `401` responses and redirects to login.
- **Graceful degradation:** App treats user as unauthenticated and redirects appropriately.

---

## 4. How does using a ProtectedRoute improve the user experience and security of the application?

### User Experience:
- **Seamless redirects:** Users are automatically redirected to login when accessing protected content.
- **Loading states:** Shows loading spinner while checking authentication.
- **Consistent behavior:** All protected routes behave the same way.

### Security:
- **Access control:** Prevents unauthorized users from accessing protected components.
- **Route protection:** Works at the routing level, not just UI level.
- **Centralized security:** All route protection logic is in one reusable component.
- **Prevents data leakage:** Components never render for unauthenticated users.

---

## 5. What are the security implications of showing different UI elements based on token state? Could this ever leak information?

### Security Implications:
- **Information disclosure:** UI elements themselves might reveal sensitive information about app structure.
- **Client-side security:** Authentication checks on the client are for UX only, not security.
- **API security:** Real security must be enforced server-side on every API call.

### Potential risks:
- **Source code exposure:** Attackers can see all possible UI states in the client code.
- **Role enumeration:** Different UI elements might reveal available roles or permissions.
- **Feature discovery:** Hidden features might be discoverable through code analysis.

### Mitigation strategies:
- **Server-side validation:** Always validate permissions on the server.
- **Minimal exposure:** Only show UI elements that are truly necessary.
- **Progressive disclosure:** Load sensitive UI components only after server confirmation.
- **Code obfuscation:** While not foolproof, it makes analysis harder.
