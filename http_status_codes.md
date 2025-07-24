# HTTP Status Code Deep Dive for Security Work

> For web pentesters, CTF players, and curious tech gremlins.

---

## 🔍 What Are HTTP Status Codes?

When you request a resource from a web server (like typing a URL into a browser), the server responds with an HTTP status code. This is a three-digit number that tells the client (you) what happened:

- Was it successful?
- Did something break?
- Were you redirected?

Security testers care a lot about status codes because they reveal how a site behaves — and sometimes, misbehaves.

---

## 🔢 Status Code Categories

| Code Range | Category      | Meaning                          |
| ---------- | ------------- | -------------------------------- |
| 1xx        | Informational | Processing, not final            |
| 2xx        | Success       | You got what you asked for       |
| 3xx        | Redirection   | You're being sent somewhere else |
| 4xx        | Client error  | You messed up                    |
| 5xx        | Server error  | They messed up                   |

---

## ✅ 2xx – Success Responses

### 200 OK

- The resource exists and you’re allowed to see it.
- Common for: `GET /`, `GET /index.html`
- Seen in Gobuster when a path works.

```http
GET /admin HTTP/1.1
→ 200 OK
```

### 201 Created

- Something was successfully created (usually after a POST or PUT).

### 204 No Content

- Server processed the request but returns no body.
- Seen in APIs after a DELETE.

---

## 🔄 3xx – Redirection

### 301 Moved Permanently

- The resource has moved for good. Check the `Location:` header.
- Usually happens when `/admin` becomes `/admin/`

```http
GET /admin HTTP/1.1
→ 301 Moved Permanently
Location: /admin/
```

### 302 Found / 307 Temporary Redirect

- Temporary redirect.
- Can signal login flow redirection (e.g., kicked to `/login`).

Useful for:

- Detecting auth pages
- Seeing how cookies and sessions work

---

## 🚫 4xx – Client Errors

### 400 Bad Request

- Your request is malformed.
- Common in testing input validation.

### 401 Unauthorized

- You need to log in.
- Auth is required, and credentials are missing or wrong.

```http
GET /api/admin HTTP/1.1
→ 401 Unauthorized
WWW-Authenticate: Basic realm="restricted"
```

### 403 Forbidden

- Access is blocked **even if you are authenticated**.
- This is important: it means something **exists**, but you can't have it.
- Seen in Gobuster when a real path is there, but access is restricted.

```bash
Found: /admin (Status: 403)
```

### 404 Not Found

- Resource doesn’t exist.
- Normal if you're guessing paths — but see `--wildcard` in Gobuster for custom 404s that lie.

### 405 Method Not Allowed

- The HTTP method isn’t supported for that URL (e.g., `PUT` to `/login`).
- Can indicate admin-only or API functionality.

### 429 Too Many Requests

- Rate limiting is active.
- Seen when brute-forcing too quickly.
- Fix: slow down or use a proxy with delay.

---

## 💥 5xx – Server Errors

### 500 Internal Server Error

- Server-side bug. Often seen when inputs break things.
- Can be **extremely useful** in testing input validation.

### 502 Bad Gateway / 503 Service Unavailable / 504 Gateway Timeout

- Upstream or WAF-related failures.
- Might signal:
  - Reverse proxy issues
  - Temporary block
  - WAF rate limit or filtering

---

## 🛠️ Practical Security Uses

| Status Code | Why It Matters in Security                              |
| ----------- | ------------------------------------------------------- |
| 200         | You found something useful                              |
| 301/302     | Might redirect to login, staging, or hidden app version |
| 403         | Means something **is there**, but restricted            |
| 404         | Can be misleading — check for custom 404s               |
| 401         | May allow brute-forcing or bypass                       |
| 405         | Could reveal available methods on API endpoints         |
| 500+        | Input might be crashing the server — log review needed  |
| 429         | Tells you when you're being rate-limited                |

---

## 🧪 Example: Brute-Forcing Hidden Paths with Gobuster

```
gobuster dir -u http://target.site -w list.txt -s 200,403,401,500
```

Useful interpretations:

- 200 = File/folder exists
- 403 = Exists but blocked
- 401 = Requires auth (login or token?)
- 500 = Broken app or input handling issue

---

## 🔗 External References

- MDN Web Docs: [HTTP Response Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- OWASP Testing Guide: [Testing for HTTP Methods and Responses](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_HTTP_Methods.html)

---

TODO: Pair this with a logging proxy like **Burp Suite**, or check how headers change with tools like **httpie**, **curl**, or **mitmproxy**.

---


