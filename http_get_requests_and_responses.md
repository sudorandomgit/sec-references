# 📘 HTTP GET Request Response Basics: Examples + Notes

Explores raw HTTP GET requests and responses intercepted while browsing [httpbin.org](http://httpbin.org) via BurpSuite proxy intercept. We'll examine two real examples:

1. A manual visit to /get  
2. An automatic browser request to /favicon.ico

Each line is broken down with what it does, why it’s there, and how it fits into broader web behavior.

---

## 🔹 Example 1: Manual Visit to `http://httpbin.org/get` 

### 📤 Request:

```
GET /get HTTP/1.1
Host: httpbin.org
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:141.0) Gecko/20100101 Firefox/141.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Priority: u=0, i
``` 

### 🔍 Request Breakdown:

`GET /get HTTP/1.1`

* This is the request line: method, path, and protocol.
* **GET** is the method. We’re requesting data.
* **/get** is the path. If it were just /, that’s the homepage.
* This is where tools like GoBuster inject paths from a wordlist to brute-force directories.
* **HTTP/1.1** is the protocol version. Modern browsers still default to 1.1 for proxy visibility. N ewer ones like `HTTP/2` and `HTTP/3` use binary and aren’t human-readable, and behave differently in multiplexing and performance

**Q: What if nothing is after the slash?** → The request becomes GET / HTTP/1.1

**Q: What if we used HTTP/2 instead?** → We'd see binary framing instead of headers.

`Host: httpbin.org`

* Tells the server what hostname you're targeting.
* Important when multiple virtual hosts/domains share an IP address.
* Tools like Gobuster in `vhost` mode change this value to find subdomains.

`User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:141.0) Gecko/20100101 Firefox/141.0`

* Identifies browser and platform/OS.
* Servers can change layout, features, or block you based on this.
* You can spoof this to:
	- Access mobile-only pages
	- Avoid bot protections
	- Evade basic WAF filters
	- Impersonate other devices (e.g, iPhone, Googlebot).

**Q: What is Gecko?** → rendering engine used by Firefox. Use [DeviceAtlas](https://deviceatlas.com/blog/list-of-user-agent-strings) to find common spoof strings.

`Accept: text/html,...;q=0.9,*/*;q=0.8`

* Lists content types the client is okay receiving.
* `q=0.9` and `q=0.8` are preference weights (quality values). Higher means more preferred.

**Q: Why did we get JSON when it wasn't mentioned?** → `*/*` = fallback, accept anything. We got it because `*/*` was allowed.

**Q: Why semicolons?** → HTTP header syntax. ; introduces parameters like q.

`Accept-Language: en-US,en;q=0.5`

* Preferred language. If a site offers multi-language support, the server might redirect or respond accordingly.
* `q=0.5` makes `en` a fallback.

`Accept-Encoding: gzip, deflate, br`

* Indicates acceptable compression formats. 
* `gzip` is most common. `br` is Brotli (more efficient), common on modern sites.
* Server will compress response if able.
* You can modify or remove this to force an uncompressed response.

**Q: What happens if we recieve content in other encoding?** → Anything not listed is rejected.

**Q: What happens if omit this header?** →  Any content-coding is considered acceptable by the user agent. In practice however, it seems like most commonly used servers (e.g. Apache, nginx) will not do this, and will send an uncompressed response if the field is omitted. 

`Connection: keep-alive`

* Reuse the same TCP connection for performance.
* Alternative: `close`, which tells the server to drop the connection afterward.

`Upgrade-Insecure-Requests: 1`

* Tells the server this client prefers `https://` over `http://` if both are available.
* Browser-initiated behavior - related to automatic HTTPS upgrades in browsers.
* Values are `1` (yes) or nothing.

`Priority: u=0, i`

* A non-standard header sometimes used in HTTP/2, priority hint.
* Low urgency `u=0` for non-critical background fetches. `u` can go upto 6
* `i` stands for incremental rendering — the client prefers to start rendering before receiving all content.
* This can be spoofed or changed to request high-priority content in some attack scenarios.

### 📥 Response:

```
HTTP/1.1 200 OK
Date: Thu, 24 Jul 2025 06:49:43 GMT
Content-Type: application/json
Content-Length: 586
Connection: keep-alive
Server: gunicorn/19.9.0
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
Via: HTTP/1.1 m_proxy_che2
Via: HTTP/1.1 s_proxy_che2

{
  "args": {},
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
    "Accept-Encoding": "gzip, deflate",
    "Accept-Language": "en-US,en;q=0.5",
    "Host": "httpbin.org",
    "Priority": "u=0, i",
    "Upgrade-Insecure-Requests": "1",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:141.0) Gecko/20100101 Firefox/141.0",
    "X-Amzn-Trace-Id": "Root=1-6881d786-37e85bbd04ecfb4a1f1a06ce",
    "X-Proxyuser-Ip": "14.194.95.250"
    <... all headers echoed back ...>
  },
  "origin": "14.194.95.250, 155.190.5.19",
  "url": "http://httpbin.org/get"
}
```

### 🔍 Response Breakdown:

`HTTP/1.1 200 OK`

* The status line of the response. `200 OK` means the request was successful.
* The `HTTP/1.1` part indicates the protocol version used by the server.
* If the status code were different (e.g., `404`, `500`, `301`), that would change the meaning of the response entirely.

`Date: Thu, 24 Jul 2025 06:49:43 GMT`

* Tells you when the server generated the response.
* Often used for logging, caching, or debugging time drift between client and server.

`Content-Type: application/json`

* Indicates the media type of the response body.
* This helps the browser or client decide how to parse or display the data.
* This is how you know you're dealing with JSON, not HTML, XML, or an image.

`Content-Length: 586`

* The size of the response body in bytes.
* Used by the client to allocate memory or buffer space.
* Also lets the client know when the full response has been received.

**Q: What happens when content length foes not match the header?** → 

`Connection: keep-alive`

* Tells the client that the server will keep the TCP connection open after this request.
* This is a performance optimization — it reduces the cost of setting up new connections.

`Server: gunicorn/19.9.0`

* Identifies the backend HTTP server software used to generate the response.
* Can be useful for fingerprinting and vulnerability scanning.
* `gunicorn` is a Python WSGI HTTP server often used with Flask or Django apps.

**Q:WHat are some common server types? Does client side handling differ based on this header?** →

`Access-Control-Allow-Origin: *`

* A CORS header indicating that any domain is allowed to access the resource.
* Public APIs use this a lot.
* This value must be more restrictive (`https://example.com`) if credentials (like cookies or headers) are involved.

`Access-Control-Allow-Credentials: true`

* Tells the browser that cookies, authorization headers, or TLS client certificates are allowed in cross-origin requests.
* Only works if the `Allow-Origin` is not `*` — otherwise it will be ignored by the browser.

`Via: HTTP/1.1 m_proxy_che2` 
`Via: HTTP/1.1 s_proxy_che2`

* These headers are injected by intermediate proxies.
* They document the path the response took from server to client.
* Useful in enterprise networks, CDNs, and debugging proxy chains.

#### Response Body

* The JSON response includes echo info:

  * `args`: any query parameters passed in the request (empty here).
  * `headers`: the headers the server received.
  * `origin`: your IP address and possibly a proxy’s IP too.
  * `url`: the final URL that was requested.

---


## 🔹 Example 2: Auto-Fetched favicon.ico Request 

### 📤Request:

```
GET /favicon.ico HTTP/1.1
Host: httpbin.org
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:141.0) Gecko/20100101 Firefox/141.0
Accept: image/avif,image/webp,image/png,image/svg+xml,image/*;q=0.8,*/*;q=0.5
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://httpbin.org/
Sec-Fetch-Dest: image
Sec-Fetch-Mode: no-cors
Sec-Fetch-Site: cross-site
Priority: u=6
Te: trailers
Connection: keep-alive
```

### 📥Response: 

Non critical - often 404 or small image returned

```
HTTP/1.1 404 NOT FOUND
Date: Thu, 24 Jul 2025 06:49:45 GMT
Content-Type: text/html
Content-Length: 233
Connection: keep-alive
Server: gunicorn/19.9.0
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
Via: HTTP/1.1 m_proxy_che2

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>404 Not Found</title>
<h1>Not Found</h1>
<p>The requested URL was not found on the server.  If you entered the URL manually please check your spelling and try again.</p>
```

### 🔍 Request Breakdown:

**Q: Why was **`/favicon.ico`** requested?** → Browsers auto-request this file to display a tab icon. It’s not user-triggered.

**Q: What is a favicon?** → Short for “favorite icon.” Place a `favicon.ico` file in your site root to use it, or declare a different icon in HTML.

`GET /favicon.ico HTTP/1.1`

* Automatic request by browser to fetch website tab icon.

`Accept: image/avif,image/webp,image/png,image/svg+xml,image/*;q=0.8,*/*;q=0.5`

* Unlike the `/get` request, this one is asking for images only.
* Favicons come in many formats: `.ico`, `.png`, `.svg`, etc.
* The browser tells the server it’s willing to accept any image, in order of preference.

`Accept-Language: en-US,en;q=0.5`  
`Accept-Encoding: gzip, deflate, br`

* Language preference stays the same.
* Again, browser is happy to accept compressed responses.

`Referer: http://httpbin.org/`

* Tells the server where this request came from.
* Can be used by servers: 
	- to verify request context
	- to implement basic CSRF protection
	- for logging and analytics
	- for access control (sometimes overly reliant)

`Sec-Fetch-Dest: image`

* Specifies the resource type: `image`, `script`, `style`, `iframe`, etc.
* Helps the server understand what the browser intends to do with the response.

`Sec-Fetch-Mode: no-cors`

* Indicates that the browser is making a simple, unauthenticated request and doesn't expect cookies or sensitive data.
* `no-cors` is typical for resources like favicons, fonts, and images.

`Sec-Fetch-Site: cross-site`

* The page making the request (httpbin.org) is different from the current origin (e.g., localhost or another domain).
* Helps the server enforce CORS and determine risk of CSRF or misuse.

`Priority: u=6`

* Higher urgency = Higher priority = load this quickly.
* Browsers prioritize favicons to display the UI promptly.
* Not a security-relevant header, but interesting to observe.

`Te: trailers`

* Part of chunked (legacy) encoding from HTTP/1.1. 
* Means “I support chunked encoding with trailing metadata”
* Almost no one uses this anymore. Ignore it unless you're into ancient HTTP relics.

`Connection: keep-alive`

* Again, allows the TCP connection to remain open.

#### ➕ Expanded Explanation: `Sec-Fetch-*` Headers

**What does "Sec-Fetch" stand for?**
Security Fetch Metadata — a set of headers browsers automatically include to give servers context about the request.

**They help answer:**

* Is this request part of a navigation event or a resource fetch?
* Was it user-initiated or a background script?
* Is this site trying to pull data it shouldn't have access to?

**Breakdown of the family:**

* `Sec-Fetch-Dest`: What type of resource is being fetched?

  * Common values: `document`, `image`, `style`, `script`, `iframe`, `empty`, `font`, `audio`, `video`

* `Sec-Fetch-Mode`: What level of CORS enforcement is being used?

  * `navigate`: Full page navigation
  * `cors`: Same-origin or CORS-fetched resource
  * `no-cors`: External resource like a script or image
  * `same-origin`: Only allows same-origin fetches

* `Sec-Fetch-Site`: How "related" is the request to the origin?

  * `same-origin`: Exact match (domain + protocol + port)
  * `same-site`: Subdomain match (e.g. `foo.example.com` to `bar.example.com`)
  * `cross-site`: Totally unrelated domain
  * `none`: From browser UI (e.g. bookmarks)

These headers are **automatically added by modern browsers**. You can spoof them with tools like Burp or curl, but servers increasingly rely on them to make access control decisions.

---
### 🌍 What's CORS?

**CORS = Cross-Origin Resource Sharing**\
It’s a browser-enforced security feature that governs how resources can be requested between different origins.

#### 🚧 What is an origin?

An origin = `scheme` + `host` + `port`.

- `https://example.com` and `http://example.com` → different origins (because of scheme)
- `https://example.com:443` and `https://example.com:8443` → different origins (because of port)

#### 🔄 Why does CORS matter?

CORS controls which websites can make requests to other sites **via JavaScript**. For example, `evil.com` should not be able to read a logged-in user's data from `bank.com`.

Without CORS, a malicious site could abuse your credentials (cookies, headers) to access private APIs silently.

#### 🔑 Key CORS Headers

###### `Access-Control-Allow-Origin`

* Says who is allowed to make cross-origin requests.
* `*` = open to everyone (but can't be combined with credentials).
* Specific domains are more secure: `https://myapp.example.com`

###### `Access-Control-Allow-Methods`

* Specifies allowed HTTP methods for the resource.
* Common values: `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS`
* Especially important for preflighted requests (non-simple requests).

###### `Access-Control-Allow-Headers`

* Controls which custom headers the browser is allowed to send.
* If your frontend uses `Authorization` or `X-Custom-Header`, they must be listed here.

###### `Access-Control-Allow-Credentials`

* Set to `true` if the server wants to allow credentials (cookies, Basic auth) in cross-origin requests.
* Only works when `Access-Control-Allow-Origin` is specific (not `*`).

###### `Access-Control-Expose-Headers`

* Allows the browser to access certain response headers that are otherwise hidden.
* Example: exposing a `Location` header for a redirected request.

###### `Access-Control-Max-Age`

* Time in seconds for how long a preflight request can be cached.
* Helps reduce the number of CORS preflight OPTIONS requests.

#### 🧪 Preflight Requests

* Sent automatically when:
	- Using a method other than `GET`, `HEAD`, `POST`
	- Using custom headers (like `Authorization`)
	- Using non-simple content types (like `application/json`)
* Browser sends an `OPTIONS` request first to ask permission.
* If server says yes → real request is sent.

#### 👀 Security Implications

CORS mistakes can be devastating:

* Misconfigured wildcards can expose private APIs
* Forgetting credentials rules can break auth
* Failing to include proper methods can block legitimate clients

---

### Summary

This guide explores how basic HTTP GET requests are structured and interpreted — including default browser behaviors like favicon fetching and language negotiation. It lays the groundwork for manipulating and replaying requests effectively.

There is no one-size-fits-all for HTTP headers — each one carries implications for routing, caching, authentication, and cross-site behaviors. GET requests seem deceptively simple but are loaded with implicit behaviors depending on client, server, network, and browser policy. From automatic favicon fetching to compression preferences and origin policy enforcement, every line in a request and response is a signal, hint, or negotiation with the server.

CORS headers like `Access-Control-Allow-Origin` define what cross-domain scripts can do — critical in API security. HTTP/1.1 persists connections by default, but versioning affects cacheability and parsing. Proxies inject `Via` headers for visibility, and the server stack itself (like `gunicorn`) reveals deployment details that could be probed.

This isn’t the end of understanding HTTP — it’s the beginning. Once you're solid on GETs, you move into POSTs, PUTs, and DELETEs. Then on to cookies, sessions, tokens, headers that hint at features or bugs (`X-Forwarded-For`, `ETag`, `Cache-Control`), and attack surfaces like CSRF, XSS, and SSRF that rely on this foundation.

Keep watching the wire. Your browser’s doing 50 things in the background, and the server is guessing what you mean based on this very request.

Next up: GET parameters, query strings, or custom header tampering.
Let’s build this into a series. Next up: POST requests and form manipulation?
