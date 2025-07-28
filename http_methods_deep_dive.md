# HTTP Methods Guide for Security Testing

> Clear explanations, use cases, and examples for HTTP methods meaningful in pentests and recon.

---

## 🛠️ Core Concepts: Safe vs Idempotent

HTTP methods are classified by two properties:

- **Safe**: Shouldn't change server state (GET, HEAD, OPTIONS, TRACE)
- **Idempotent**: Repeating them has same result (GET, HEAD, PUT, DELETE, OPTIONS, TRACE) though some can change state once

---

## 🔄 GET

- **Use**: Retrieve data; safe & idempotent
- **Security Notes**: Don’t rely on it to hide things—some apps trigger actions on GET (bad practice)

### 🧪 Example

```bash
curl -i http://target.com/page?id=123
```

**Response**

```
HTTP/1.1 200 OK
Content-Type: text/html
...
<html>...</html>
```

---

## 🔁 HEAD

- **Use**: Like GET but only headers—no body
- **Security Notes**: Great for checking existence, size, or custom 404 behind `--wildcard`

### 🧪 Example

```bash
curl -I http://target.com/secret
```

**Response**

```
HTTP/1.1 403 Forbidden
Content-Type: text/html
Content-Length: 345
```

---

## 📨 POST

- **Use**: Submit data (forms, login, file uploads); not safe nor idempotent
- **Security Notes**: Check for CSRF, injection, unsafe redirects

### 🧪 Example

```bash
curl -i -X POST -d "user=admin&pass=123" \
  http://target.com/login
```

**Response**

```
HTTP/1.1 302 Found
Location: /dashboard
```

---

## ✏️ PUT

- **Use**: Replace or create a resource; idempotent but not safe
- **Security Notes**: Unexpected open PUT can lead to file upload/backdoor risk

### 🧪 Example

```bash
curl -i -X PUT --data-binary "@backdoor.jsp" \
  http://target.com/uploads/backdoor.jsp
```

**Response**

```
HTTP/1.1 201 Created
```

---

## 🔄 PATCH

- **Use**: Partial update; not safe/not idempotent
- **Security Notes**: Check access controls; lower use than PUT

---

## 🗑 DELETE

- **Use**: Remove resource; idempotent but not safe
- **Security Notes**: Unauthorized DELETE can lead to full site deletion

### 🧪 Example

```bash
curl -i -X DELETE http://target.com/data/42
```

**Response**

```
HTTP/1.1 204 No Content
```

---

## 🔧 OPTIONS

- **Use**: Get supported methods via `Allow:` header
- **Security Notes**: Can leak unsupported methods (e.g., TRACE, PUT)

### 🧪 Example

```bash
curl -i -X OPTIONS http://target.com/
```

**Response**

```
HTTP/1.1 200 OK
Allow: GET, POST, OPTIONS, HEAD
```

---

## 🔄 TRACE

- **Use**: Echoes request for debugging; safe/idempotent
- **Security Notes**: Cross-Site Tracing (XST) can expose cookies — usually disable it

---

## 🔐 CONNECT

- **Use**: Opens TCP tunnel through proxy
- **Security Notes**: Misconfigured servers may let you proxy internal network

---

## ✅ Example Matrix

| Method  | Safe | Idempotent | Common Responses        | Security Relevance                              |
| ------- | ---- | ---------- | ----------------------- | ----------------------------------------------- |
| GET     | ✔️   | ✔️         | 200, 301, 403, 404, 500 | Baseline enumeration, injection via params      |
| HEAD    | ✔️   | ✔️         | 200, 403, 404           | Headless checks—stealthier than GET             |
| OPTIONS | ✔️   | ✔️         | 200 + `Allow` header    | Learn which methods are enabled/misconfigured   |
| POST    | ❌    | ❌          | 200, 201, 302, 401      | Test form auth, inject payloads                 |
| PUT     | ❌    | ✔️         | 201, 403, 405           | Detect arbitrary file upload                    |
| PATCH   | ❌    | ❌          | 200, 204, 403, 405      | Partial changes—test ACL or fortification flaws |
| DELETE  | ❌    | ✔️         | 204, 403, 405           | Test destructive capability                     |
| TRACE   | ✔️   | ✔️         | 200 + echoed request    | Check for XST exposure                          |
| CONNECT | ❌    | ❌          | 200                     | Check for open proxy misuse                     |

---

## 🔍 Testing Methods: Quick Commands

```bash
# Check all methods
curl -i -X OPTIONS http://target.com/ | grep Allow

# Test unsupported methods
curl -i -X TRACE http://target.com/
curl -i -X DELETE http://target.com/resource
curl -i -X PUT --data "data" http://target.com/newfile
```

---

## 🔗 Further Reading

- MDN: [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- freeCodeCamp: [Learn HTTP Methods](https://www.freecodecamp.org/news/learn-http-methods-like-get-post-and-delete-a-handbook-with-code-examples)
- OWASP: [Testing for HTTP Methods](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_HTTP_Methods.html)
- RESTful API Tutorial: [HTTP Methods](https://restfulapi.net/http-methods/)
- Security StackExchange: [Security concerns with PUT, DELETE, TRACE](https://security.stackexchange.com/questions/225325/is-the-http-method-options-secure-nowadays)

---

🎯 Next steps:

- Practice with `curl` or **Burp Repeater**
- Combine this with your status codes guide to make informed requests
- Try the next TryHackMe room that focuses on web exploitation or API fuzzing

