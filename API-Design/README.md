# API Design & System Design Notes

Today I learned about API Design in System Design — how good APIs are designed, why naming matters, RESTful design, optimization, pagination, side effects, consistency, and real-world backend architecture concepts.

## What is an API?

API (Application Programming Interface) is a contract between systems.

It tells:
- what function/service is available
- what input to send
- what output comes back
- what errors can happen

API does NOT explain:
- internal code
- database logic
- implementation details
### Simple Example

Suppose WhatsApp provides this API:

```
GET /groups/123/admins
```

This means:
- Fetch admins
- of group 123

Response:

```json
{
  "admins": [
    {
      "id": 1,
      "name": "Akhilesh"
    }
  ]
}
```
## Main Goal of Good API Design

A good API should be:
- Clear
- Predictable
- Easy to understand
- Easy to maintain
- Scalable
- Small and focused
## 1. API Naming Matters

**Bad API:**
```
GET /getAdmins
```

Why bad?
- HTTP GET already means fetching data
- get becomes redundant

**Better:**
```
GET /admins
```

**Even Better:**
```
GET /groups/123/admins
```

This clearly represents:
- resource hierarchy
- ownership relation
## 2. RESTful API Design

REST means:
- URL should represent resources
- HTTP methods should represent actions

### HTTP Methods

| Method | Meaning |
|--------|---------|
| GET | Fetch data |
| POST | Create data |
| PUT | Replace data |
| PATCH | Update partial data |
| DELETE | Remove data |
### Good REST Examples

**Employees**
```
GET /employees
GET /employees/5
DELETE /employees/5
```

**Leave Management System**
```
GET /employees/5/leaves
POST /leave-requests
```

### Bad REST Examples
```
GET /getEmployee?id=5
POST /doEverything?action=createLeave
```

Problems:
- hard to scale
- hard to understand
- messy routing
3. Routing vs Action

I learned that routes should describe resources, not actions.

❌ Bad
/chat?action=getAdmins

Why bad?

endpoint becomes generic
one route handles many actions
difficult to maintain
✅ Better
/groups/123/admins

Clear and readable.

4. Query Params vs Path Params
Path Params

Used for:

identifying specific resource

Example:

/employees/5
/groups/123/admins
Query Params

Used for:

filtering
sorting
pagination

Example:

/employees?page=1&limit=10
/employees?department=HR
5. API Response Should Be Small

Bad API Response:

{
  "admins": [...],
  "group": {...},
  "settings": {...},
  "messages": [...],
  "analytics": {...}
}

Why bad?

unnecessary network usage
confusing
difficult maintenance
Better Response
{
  "admins": [...]
}

Return only required data.

6. Avoid Unnecessary Parameters

Bad:

getAdmins(groupId, role, country, settings)

Too many unrelated parameters.

Better:

getAdmins(groupId)

Keep APIs focused.

7. API Optimization (Very Important)

I learned that sometimes frontend/client can send extra information to reduce backend work.

Normal Flow

Frontend sends:

{
  "employeeId": 5
}

Backend fetches:

employee
department
manager
permissions

Multiple DB calls happen.

Optimized Flow

Frontend already knows some data, so it sends:

{
  "employeeId": 5,
  "departmentId": 2,
  "managerId": 10
}

Now backend can skip some database queries.

This reduces:

DB calls
network traffic
latency
Important Learning

Optimization should ONLY be done when:

performance is critical
traffic is huge
system is under heavy load

Otherwise APIs become:

tightly coupled
confusing
risky

Backend should still validate critical information.

Never trust frontend completely.

Real-World Example

Apps like:

Netflix
Facebook
Uber

optimize APIs heavily because saving even 10ms matters at scale.

8. Side Effects in APIs

One API should do ONE main responsibility.

❌ Bad Example
setAdmins(groupId, admins)

Internally it:

creates group
adds members
assigns admins

Very confusing.

Problems
hidden behavior
difficult testing
unexpected results
atomicity issues
Better Design
POST /groups
POST /groups/123/members
POST /groups/123/admins

Separate responsibilities.

9. Atomicity

Atomicity means:

Either:

everything succeeds
OR
everything fails

No partial state.

Example:

group created
but admin assignment failed

This creates inconsistent system state.

10. Pagination

Large responses should be divided into smaller chunks.

Example
GET /employees?page=1&limit=10

Benefits:

faster APIs
less memory
scalable systems
11. Fragmentation

Internal services may split huge responses into packets/fragments.

Example:

Packet 1
Packet 2
Packet 3
End Packet

Used mostly in microservice communication.

12. Data Consistency

I learned that APIs must balance:

consistency
speed
Example

User requests admins.

At same time:
someone adds another admin.

Response may return:

2 admins
even though DB now has 3.
Strong Consistency

Always latest data.

Pros:

accurate

Cons:

slower
Eventual Consistency

Slightly old data acceptable.

Useful for:

comments
likes
notifications
feeds
13. Caching

Instead of hitting DB every time:

Request → Cache

Benefits:

faster response
reduced DB load

Tradeoff:

data may be slightly stale
14. Service Degradation

Under heavy traffic:
instead of full data,
system may return essential data only.

Example:

Instead of:

{
  "name": "Akhilesh",
  "profilePhoto": "...",
  "bio": "...",
  "followers": 1000
}

return:

{
  "name": "Akhilesh"
}

This helps systems survive high load.

My Biggest Doubts I Learned Today
Doubt 1:

Why send extra info from frontend?

Answer:
To reduce backend DB calls and improve performance.

Doubt 2:

Should backend trust frontend data?

No.

Backend must validate important information.

Doubt 3:

Why /groups/123/admins is better than ?groupId=123?

Because path params represent resource hierarchy more clearly.

Doubt 4:

Why /chat?action=getAdmins is bad?

Because one endpoint becomes overloaded with many actions and becomes difficult to maintain.

Final Key Takeaways
Good API design is very important in system design
RESTful APIs are cleaner and scalable
One API should have one responsibility
Avoid side effects
Keep responses small
Use pagination for huge data
Optimize carefully
Design APIs for readability and maintainability
Backend architecture matters more at scale
Realization

Today I understood that API Design is not just writing endpoints.

It involves:

scalability
readability
maintainability
performance
consistency
architecture thinking

This is one of the core skills of a good software engineer.