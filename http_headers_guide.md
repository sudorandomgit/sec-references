# HTTP Headers Guide for Security Testing

> A practical breakdown of headers worth watching, poking, and exploiting.

---

## 🧠 What Are HTTP Headers?

Headers are metadata in HTTP requests and responses. They're the boring stuff… until they leak secrets, break logins, or let you hijack a session.

Headers are **key-value pairs** that:

- Describe content (`Content-Type: application/json`)
- Handle auth (`Authorization: Bearer abc123`)
- Control caching, cookies, redirects
- Reveal tech, behavior, and mistakes

---

## 📥 Request Headers

These are sent *by you* (or your tools).

### 🔐 Authorization

```http
Authorization: Basic dXNlcjpwYXNz
Authorization: Bearer <JWT_TOKEN>
```

- Carries credentials or tokens
- Try replaying with invalid tokens or tampered JWTs

### 🍪 Cookie

```http
Cookie: session=eyJhbGciOi...
```

- Includes session tokens
- Intercept and inspect these for leakage, tampering

### 🧠 User-Agent

```http
User-Agent: Mozilla/5.0 (X11; Linux x86_64)
```

- Used for analytics, fingerprinting, access control
- Spoof it to mimic mobile clients or bypass filters

### 🕵️‍♀️ X-Forwarded-For

```http
X-Forwarded-For: 127.0.0.1
```

- Shows real or spoofed client IP
- Test if the app trusts IP from this header (bad idea!)

### 🌍 Referer

```http
Referer: https://target.com/login
```

- Tells where the user came from
- Can leak secrets in URLs

---

## 📤 Response Headers

These come *from the server* — your recon treasure map.

### 🕸 Server / X-Powered-By

```http
Server: Apache/2.4.1 (Unix)
X-Powered-By: PHP/7.3.2
```

- Reveals software + version
- Useful for vuln hunting

### 🍪 Set-Cookie

```http
Set-Cookie: sessionid=abc123; HttpOnly; Secure
```

- Controls cookies
- Look for missing `HttpOnly`, `Secure`, or `SameSite` flags

### 🔒 Strict-Transport-Security

```http
Strict-Transport-Security: max-age=63072000; includeSubDomains
```

- Enforces HTTPS
- Missing = downgrade attack possible

### 🛑 X-Frame-Options

```http
X-Frame-Options: DENY
```

- Prevents clickjacking
- Missing = UI redress attacks possible

### 🧱 Content-Security-Policy (CSP)

```http
Content-Security-Policy: default-src 'self'
```

- Restricts what can run on the page (JS, fonts, etc.)
- Weak or missing = XSS playground

### 🚫 X-Content-Type-Options

```http
X-Content-Type-Options: nosniff
```

- Stops MIME-type sniffing
- Missing = browser might interpret a JPEG as JS (seriously)

### 🔄 Access-Control-Allow-Origin (CORS)

```http
Access-Control-Allow-Origin: *
```

- Controls cross-origin access
- `*` is dangerous — test cross-site GETs/POSTs

---

## 🧪 Useful Tests

- Use `curl -I` or Burp to view headers
- Try removing cookies or auth headers and resending
- Spoof `Referer`, `User-Agent`, or `X-Forwarded-For`
- Look for missing `Secure`, `HttpOnly`, or CSP
- Check CORS headers on APIs

---

## 📚 Further Reading

- MDN: [HTTP Headers Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
- OWASP Secure Headers Project: [Cheat Sheet](https://owasp.org/www-project-secure-headers/)
- PortSwigger: [HTTP Header Security Guide](https://portswigger.net/web-security/http-headers)

---

💡 Most headers are there to help. But if they're wrong — they *really help you*. So check every one.

---

🎯 Next idea: Use `curl -v`, `httpx`, or Burp Suite and build a header recon checklist per target.

