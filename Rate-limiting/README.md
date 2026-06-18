# Rate-limiting for API 


## What is Rate Limiting?

Rate Limiting is a technique used to control how many requests a client can send to an API within a specific period of time.

It helps protect the server from:
- Abuse
- Too many requests
- Denial of Service (DoS) attacks
- Resource exhaustion
- Performance degradation

---

## Simple Definition

> Rate Limiting = Limiting the number of API requests allowed within a fixed time.

Example:

```
10 requests per 60 seconds
```

A client can call the API **10 times in one minute**.

The **11th request** will be rejected.

---

## Real-Life Example

Imagine an ATM.

You can withdraw money only **5 times per day**.

If you try a 6th withdrawal:

```
Transaction Rejected
Daily Limit Exceeded
```

Rate limiting works the same way.

---

## API Example

Rate Limit:

```
100 requests per minute
```

Requests:

```
Request 1  ✅
Request 2  ✅
...
Request 100 ✅
Request 101 ❌
```

Response:

```http
HTTP/1.1 429 Too Many Requests
```

---

## Why Do We Use Rate Limiting?

- Prevent API abuse
- Protect backend servers
- Prevent DoS attacks
- Ensure fair usage
- Improve performance
- Reduce server overload

---

## Important Parameters

### rate

Maximum number of requests allowed.

Example:

```
rate = 10
```

Means:

```
Allow 10 requests
```

---

### per

Time window in seconds.

Example:

```
per = 60
```

Means:

```
Within 60 seconds
```

---

### Combined Example

```
rate = 10
per = 60
```

Meaning:

```
10 requests every 60 seconds
```

---

## HTTP Status Code

When the limit is exceeded:

```http
429 Too Many Requests
```

Example response:

```json
{
    "error": "Rate limit exceeded"
}
```

---

## Rate Limit Response Headers

Many APIs return these headers:

```
X-RateLimit-Limit
```

Maximum allowed requests.

Example:

```
100
```

---

```
X-RateLimit-Remaining
```

Remaining requests.

Example:

```
35
```

---

```
X-RateLimit-Reset
```

Time when the limit resets.

Example:

```
60 seconds
```

---

## Types of Rate Limiting

### 1. API-Level Rate Limiting

Limits all users together.

Example:

```
API Limit

1000 requests/minute
```

If all users together exceed 1000 requests:

```
429 Too Many Requests
```

---

### 2. Key-Level Rate Limiting

Limits a specific API Key or user.

Example:

```
User A

100 requests/minute
```

User A reaches 100 requests.

User A gets:

```
429
```

User B can still access the API.

---

### 3. Per-API Rate Limit

Different APIs have different limits.

Example:

```
GET /users

100 requests/minute
```

```
POST /payment

20 requests/minute
```

---

### 4. Per-Endpoint Rate Limit

Specific endpoint has its own limit.

Example:

```
POST /login

5 requests/minute
```

Useful to prevent brute-force login attacks.

---

## Example Scenario

Login API

```
POST /login
```

Rate Limit:

```
5 requests/minute
```

Attempts:

```
1 ✅
2 ✅
3 ✅
4 ✅
5 ✅
6 ❌
```

Response:

```
429 Too Many Requests
```

---

## Common Use Cases

- Login API
- OTP Verification
- Password Reset
- Payment APIs
- File Upload APIs
- Search APIs
- Public APIs

---

## Benefits

- Protects backend servers
- Prevents abuse
- Prevents DoS attacks
- Fair usage for all users
- Improves system stability
- Controls traffic

---

## As an Automation Tester

### Functional Test Cases

- Verify valid requests succeed within the limit.
- Verify the API blocks requests after the limit is reached.
- Verify status code is **429**.
- Verify correct error message.
- Verify requests work again after the reset time.
- Verify different users have independent limits (if applicable).

---

### Negative Test Cases

- Exceed rate limit.
- Send rapid consecutive requests.
- Test invalid API key.
- Test expired token.
- Test multiple endpoints with different limits.

---

### Performance Test Cases

- Verify API behavior under heavy traffic.
- Verify no server crash.
- Verify response time remains acceptable.
- Verify gateway blocks excessive traffic.

---

## Interview Questions

### What is Rate Limiting?

Rate Limiting controls the number of requests a client can send within a specific time period.

---

### Why is Rate Limiting used?

- Prevent abuse
- Prevent DoS attacks
- Protect backend
- Ensure fair usage
- Improve performance

---

### Which status code is returned when the limit is exceeded?

```
429 Too Many Requests
```

---

### What is the difference between API-Level and Key-Level Rate Limiting?

API-Level:
- Applies to all users combined.

Key-Level:
- Applies to an individual user or API key.

---

## Quick Revision

```
Rate Limiting
      │
      ▼
Controls API Requests
      │
      ├── Prevents Abuse
      ├── Prevents DoS
      ├── Protects Server
      ├── Fair Usage
      └── Improves Performance

Configuration

rate = Number of requests
per = Time window

Example

rate = 10
per = 60

Meaning:

10 requests every 60 seconds

Exceeded?

↓

HTTP 429 Too Many Requests
```

---

# One-Line Memory Trick

> **Rate Limiting = "Only a fixed number of requests are allowed within a specific time window."**