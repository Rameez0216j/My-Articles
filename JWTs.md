# The History of JWT

JSON Web Tokens (JWT) emerged in the early 2010s as part of the broader evolution in web authentication.

## Before JWT: Session-Based Authentication

Before JWTs, session-based authentication was the standard approach:

1. **Session ID Generation**: The server created a session ID.
2. **Database Storage**: The session ID was stored in a database.
3. **Client-Side Cookie**: The session ID was sent to the client as a cookie.

### Limitations of Session-Based Authentication:
- Required database lookups for each authentication request.
- Session state had to be shared across multiple servers, making scalability difficult.

## The Need for Stateless Authentication
With the rise of **mobile apps, SPAs, and distributed systems**, a more efficient, **stateless** authentication method became necessary.

### Enter JWTs
JWTs were standardized as **RFC 7519** by the **IETF** in **May 2015**. The standard defined "a compact, URL-safe means of representing claims to be transferred between two parties."

### Advantages of JWTs:
- **Stateless Authentication**: No need for server-side session storage.
- **Cross-Domain Compatibility**: Works across different services.
- **Scalability**: Reduces database load.
- **Flexibility**: Can carry any JSON-serializable data.
- **Security**: Uses digital signatures for verification.

Today, JWTs are widely used in **modern web authentication**, especially in **microservices architectures** where services verify user identity without a shared session database.

---

# The Structure of a JWT
A JWT consists of **three Base64-encoded segments**, separated by dots (`.`):

```
header.payload.signature
```

This simple structure enables JWTs to solve **complex authentication challenges** in distributed systems.

## 1. The Header
The header contains metadata about the token, typically:

```json
{
  "alg": "HS256",  // Algorithm used for signing
  "typ": "JWT"      // Token type
}
```

## 2. The Payload
The payload (also called **claims**) contains the actual data to be transmitted.

### Default Claims in a JWT:
```json
{
  "azp": "http://localhost:3000",   // Authorized party
  "exp": 1639398300,                  // Expiration time
  "iat": 1639398272,                  // Issued at
  "iss": "https://example.com",      // Issuer
  "nbf": 1639398220,                  // Not before
  "sid": "sess_12345",               // Session ID
  "sub": "user_67890"                // Subject (user ID)
}
```

### Custom Claims:
```json
{
  "user_id": "user_abcdef123456789",
  "avatar": "https://example.com/avatar.jpg",
  "full_name": "Doe Maria",
  "email": "maria@example.com"
}
```

## 3. The Signature
The signature ensures the integrity and authenticity of the JWT. It is created using:

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

### Example Code for Creating a JWT:
```javascript
const crypto = require('crypto');

function base64UrlEncode(str) {
  return Buffer.from(str).toString('base64')
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=/g, '');
}

function createJWT(header, payload, secret) {
  const encodedHeader = base64UrlEncode(JSON.stringify(header));
  const encodedPayload = base64UrlEncode(JSON.stringify(payload));
  const signatureInput = encodedHeader + '.' + encodedPayload;
  const signature = crypto.createHmac('sha256', secret)
    .update(signatureInput)
    .digest('base64')
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=/g, '');
  return `${encodedHeader}.${encodedPayload}.${signature}`;
}

const header = { "alg": "HS256", "typ": "JWT" };
const payload = {
  "azp": "http://localhost:3000",
  "exp": 1639398300,
  "iat": 1639398272,
  "iss": "https://example.com",
  "nbf": 1639398220,
  "sid": "sess_12345",
  "sub": "user_67890",
  "user_id": "user_abcdef123456789",
  "avatar": "https://example.com/avatar.jpg",
  "full_name": "Doe Maria",
  "email": "maria@example.com"
};

const secret = "supersecretkey";
const token = createJWT(header, payload, secret);
console.log(token);
```

### Example JWT Output:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhenAiOiJodHRwOi8vbG9jYWxob3N0OjMwMDAiLCJleHAiOjE2MzkzOTgzMDAsImlhdCI6MTYzOTM5ODI3MiwiaXNzIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSIsIm5iZiI6MTYzOTM5ODIyMCwic2lkIjoic2Vzc18xMjM0NSIsInN1YiI6InVzZXJfNjc4OTAiLCJ1c2VyX2lkIjoidXNlcl9hYmNkZWYxMjM0NTY3ODkiLCJhdmF0YXIiOiJodHRwczovL2V4YW1wbGUuY29tL2F2YXRhci5qcGciLCJmdWxsX25hbWUiOiJEb2UgTWFyaWEiLCJlbWFpbCI6Im1hcmlhQGV4YW1wbGUuY29tIn0.ld8fOURGSNnnDHUWqo_T4WiRCQPQPpcLQh7SyJlN3Es
```

### JWT Decoding
To verify the JWT, use **jwt.io** or a server-side library to decode and validate the token.

---

# JSON Web Key Sets (JWKS)
To verify JWTs securely, **JSON Web Key Sets (JWKS)** provide a standardized way to share public keys.

### How JWKS Works:
1. The **auth provider (e.g., Auth0, Clerk)** maintains cryptographic key pairs.
2. It **signs JWTs** using its private key.
3. The **public key** is exposed at a **JWKS endpoint** (`/.well-known/jwks.json`).
4. Services verify JWTs by fetching the **public key** and validating the signature.

JWKS ensures secure verification of JWTs without manually sharing secrets across services.

---

JWTs have revolutionized authentication, providing **stateless, scalable, and secure** identity verification. Their simplicity and power make them a fundamental building block in modern web security.
