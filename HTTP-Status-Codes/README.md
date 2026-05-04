# 📘 HTTP Status Codes – Complete Guide

HTTP status codes are standardized responses returned by a server to indicate the result of a client’s request.

---

## 🔹 1xx – Informational
Indicates the request has been received and processing continues.

### 100 Continue
Client should continue sending request body.
```http
POST /upload
→ 100 Continue
101 Switching Protocols

Server switches protocol (e.g., HTTP → WebSocket).

Upgrade: websocket
→ 101 Switching Protocols
102 Processing

Request is being processed (WebDAV).

🔹 2xx – Success

Request successfully processed.

200 OK

Standard success response.

GET /users/1
→ 200 OK
{
  "id": 1,
  "name": "Akhilesh"
}
201 Created

Resource created successfully.

POST /users
→ 201 Created
202 Accepted

Accepted but processing asynchronously.

POST /report
→ 202 Accepted
203 Non-Authoritative Information

Response modified by proxy.

204 No Content

Success with no response body.

DELETE /users/1
→ 204 No Content
205 Reset Content

Client should reset UI/form.

206 Partial Content

Partial data returned (used in streaming/downloads).

🔹 3xx – Redirection

Further action required.

300 Multiple Choices

Multiple response options available.

301 Moved Permanently

Resource permanently moved.

GET /old-url
→ 301 Moved Permanently
Location: /new-url
302 Found

Temporary redirect.

GET /login
→ 302 Found
Location: /dashboard
303 See Other

Redirect using GET method.

304 Not Modified

Use cached version.

GET /data (If-Modified-Since)
→ 304 Not Modified
305 Use Proxy (Deprecated)
307 Temporary Redirect

Same method, temporary redirect.

308 Permanent Redirect

Same method, permanent redirect.

🔹 4xx – Client Errors

Client made an invalid request.

400 Bad Request

Invalid input or missing fields.

POST /users
→ 400 Bad Request
401 Unauthorized

Authentication required.

GET /profile
→ 401 Unauthorized
402 Payment Required

Reserved for future use.

403 Forbidden

Access denied.

DELETE /admin
→ 403 Forbidden
404 Not Found

Resource not found.

GET /users/99
→ 404 Not Found
405 Method Not Allowed

Invalid HTTP method.

406 Not Acceptable

Requested format not available.

407 Proxy Authentication Required
408 Request Timeout

Client took too long.

409 Conflict

Data conflict (duplicate resource).

POST /users (duplicate)
→ 409 Conflict
410 Gone

Resource permanently removed.

411 Length Required

Content-Length header required.

412 Precondition Failed
413 Payload Too Large

Request size too large.

POST /upload
→ 413 Payload Too Large
414 URI Too Long
415 Unsupported Media Type

Wrong content format.

POST /users (XML instead of JSON)
→ 415 Unsupported Media Type
416 Range Not Satisfiable
417 Expectation Failed
418 I'm a Teapot ☕
421 Misdirected Request
422 Unprocessable Entity
423 Locked
424 Failed Dependency
425 Too Early
426 Upgrade Required
428 Precondition Required
429 Too Many Requests

Rate limit exceeded.

GET /api
→ 429 Too Many Requests
431 Request Header Fields Too Large
451 Unavailable For Legal Reasons
🔹 5xx – Server Errors

Server failed to process a valid request.

500 Internal Server Error

Generic server failure.

GET /users
→ 500 Internal Server Error
501 Not Implemented

Feature not supported.

502 Bad Gateway

Invalid response from upstream server.

Upstream = backend service behind gateway

Client → API Gateway → Backend (Upstream)

If backend fails or returns invalid data:

→ 502 Bad Gateway
503 Service Unavailable

Server overloaded or under maintenance.

504 Gateway Timeout

Upstream server didn’t respond in time.

505 HTTP Version Not Supported
506 Variant Also Negotiates
507 Insufficient Storage
508 Loop Detected
510 Not Extended
511 Network Authentication Required