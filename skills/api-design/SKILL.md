---
name: api-design
description: |
  Design and implement production-grade REST, GraphQL, and gRPC APIs following industry best practices. Embodies expertise of a Principal API Architect with 15+ years designing APIs at scale for Fortune 500 companies and high-growth startups.
  Use when: user requests API design, endpoint architecture, schema design, API versioning strategies, or needs to evaluate/improve existing APIs.
  Outputs: OpenAPI/Swagger specs, GraphQL schemas, API design documents, implementation code, and migration guides.
  Keywords: REST, GraphQL, gRPC, OpenAPI, Swagger, API versioning, rate limiting, pagination, HATEOAS, API gateway, endpoints, schemas
license: Complete terms in LICENSE.txt
---

# API Design Skill

**Expertise Level**: Principal API Architect (15+ years equivalent)

This skill embodies the methodology of world-class API design teams—from Stripe's elegantly simple REST APIs to GitHub's comprehensive GraphQL implementation—distilled into a repeatable process that produces APIs developers love.

---

## Related Skills

- **[database-design](../database-design)**: Design the data layer that backs your API
- **[security-audit](../security-audit)**: Secure your API against common vulnerabilities
- **[mcp-builder](../mcp-builder)**: Expose your API through MCP servers for LLM agents
- **[documentation](../documentation)**: Create comprehensive API documentation
- **[performance-optimization](../performance-optimization)**: Optimize API response times and throughput

---

## Core Philosophy

**APIs are products, not just interfaces.** Every API design decision should optimize for:

1. **Developer Experience (DX)**: How quickly can a developer go from zero to first successful call?
2. **Predictability**: Does the API behave consistently across all endpoints?
3. **Evolvability**: Can the API grow without breaking existing consumers?
4. **Performance**: Does the design enable efficient data access patterns?
5. **Security**: Is security built into the design, not bolted on?

---

## Process Overview

### Phase 1: Domain Analysis & Requirements

Before writing any spec, deeply understand the domain:

```markdown
## Domain Analysis Checklist

### Business Context
- [ ] What business problems does this API solve?
- [ ] Who are the primary consumers (internal teams, partners, public developers)?
- [ ] What are the expected traffic patterns (read-heavy, write-heavy, bursty)?
- [ ] What are the latency requirements (real-time, near-real-time, batch)?

### Resource Identification
- [ ] What are the core domain entities?
- [ ] What are the relationships between entities?
- [ ] What operations are needed on each entity?
- [ ] What are the access patterns (CRUD, search, aggregation)?

### Constraints
- [ ] Regulatory requirements (GDPR, HIPAA, PCI-DSS)?
- [ ] Rate limiting requirements?
- [ ] Authentication/authorization model?
- [ ] Backward compatibility requirements?
```

### Phase 2: API Style Selection

Choose the right paradigm based on use case:

| Style | Best For | Avoid When |
|-------|----------|------------|
| **REST** | CRUD operations, public APIs, broad audience | Complex queries, real-time updates |
| **GraphQL** | Complex data requirements, mobile apps, multiple consumers | Simple CRUD, caching-critical systems |
| **gRPC** | Internal microservices, high-performance, streaming | Browser clients, public APIs |
| **WebSocket** | Real-time bidirectional communication | Request-response patterns |
| **Event-Driven** | Async workflows, eventual consistency | Synchronous requirements |

---

## REST API Design

### URL Structure Philosophy

URLs are the user interface of your API. They should be:
- **Intuitive**: A developer should guess the endpoint correctly
- **Hierarchical**: Reflect resource relationships
- **Consistent**: Same patterns throughout the API

```
# Resource hierarchy patterns
/users                           # Collection
/users/{id}                      # Specific resource
/users/{id}/orders               # Sub-resource collection
/users/{id}/orders/{orderId}     # Specific sub-resource

# Query parameters for filtering, sorting, pagination
/users?status=active&sort=-created_at&page=2&limit=20

# Actions that don't fit CRUD (use sparingly)
/orders/{id}/cancel              # POST - state transition
/reports/sales/generate          # POST - long-running operation
```

### HTTP Methods Semantics

| Method | Purpose | Idempotent | Safe | Request Body | Response Body |
|--------|---------|------------|------|--------------|---------------|
| GET | Retrieve resource(s) | ✅ | ✅ | ❌ | ✅ |
| POST | Create resource / trigger action | ❌ | ❌ | ✅ | ✅ |
| PUT | Full resource replacement | ✅ | ❌ | ✅ | ✅ |
| PATCH | Partial update | ❌* | ❌ | ✅ | ✅ |
| DELETE | Remove resource | ✅ | ❌ | ❌ | Optional |
| HEAD | Get headers only | ✅ | ✅ | ❌ | ❌ |
| OPTIONS | Get allowed methods | ✅ | ✅ | ❌ | ✅ |

*PATCH can be idempotent with JSON Patch format

### Response Structure

**Consistent envelope pattern:**

```json
{
  "data": {
    "id": "user_123",
    "type": "user",
    "attributes": {
      "email": "user@example.com",
      "name": "Jane Doe",
      "created_at": "2024-01-15T10:30:00Z"
    },
    "relationships": {
      "organization": {
        "data": { "type": "organization", "id": "org_456" }
      }
    }
  },
  "meta": {
    "request_id": "req_abc123",
    "processing_time_ms": 45
  }
}
```

**Collection response with pagination:**

```json
{
  "data": [...],
  "meta": {
    "total_count": 1250,
    "page": 2,
    "per_page": 20,
    "total_pages": 63
  },
  "links": {
    "self": "/users?page=2&per_page=20",
    "first": "/users?page=1&per_page=20",
    "prev": "/users?page=1&per_page=20",
    "next": "/users?page=3&per_page=20",
    "last": "/users?page=63&per_page=20"
  }
}
```

### Error Response Design

**Errors should be actionable:**

```json
{
  "error": {
    "code": "validation_error",
    "message": "The request body contains invalid data",
    "details": [
      {
        "field": "email",
        "code": "invalid_format",
        "message": "Email must be a valid email address",
        "rejected_value": "not-an-email"
      },
      {
        "field": "age",
        "code": "out_of_range",
        "message": "Age must be between 13 and 150",
        "rejected_value": 5
      }
    ],
    "documentation_url": "https://api.example.com/docs/errors#validation_error",
    "request_id": "req_abc123"
  }
}
```

### HTTP Status Code Guide

| Code | Meaning | When to Use |
|------|---------|-------------|
| **2xx Success** | | |
| 200 OK | Success | GET, PUT, PATCH, DELETE with body |
| 201 Created | Resource created | POST creating new resource |
| 202 Accepted | Async processing started | Long-running operations |
| 204 No Content | Success, no body | DELETE, PUT with no response needed |
| **3xx Redirection** | | |
| 301 Moved Permanently | URL changed forever | API version migration |
| 304 Not Modified | Use cached version | Conditional GET with ETag |
| **4xx Client Error** | | |
| 400 Bad Request | Invalid request format | Malformed JSON, missing required fields |
| 401 Unauthorized | Authentication required | Missing or invalid token |
| 403 Forbidden | Permission denied | Valid auth but insufficient permissions |
| 404 Not Found | Resource doesn't exist | Invalid ID or path |
| 405 Method Not Allowed | Wrong HTTP method | POST to read-only endpoint |
| 409 Conflict | State conflict | Duplicate creation, version mismatch |
| 422 Unprocessable Entity | Validation failed | Business rule violations |
| 429 Too Many Requests | Rate limit exceeded | Include Retry-After header |
| **5xx Server Error** | | |
| 500 Internal Server Error | Unexpected failure | Unhandled exceptions |
| 502 Bad Gateway | Upstream failure | Dependency unavailable |
| 503 Service Unavailable | Temporary overload | Include Retry-After header |
| 504 Gateway Timeout | Upstream timeout | Slow dependency |

---

## Pagination Strategies

### Offset-Based Pagination

```
GET /items?page=3&per_page=20
GET /items?offset=40&limit=20
```

**Pros**: Simple, allows jumping to any page
**Cons**: Inconsistent results with concurrent writes, slow for large offsets

### Cursor-Based Pagination

```
GET /items?cursor=eyJpZCI6MTIzfQ&limit=20
```

**Pros**: Consistent results, efficient for large datasets
**Cons**: Can't jump to arbitrary page, cursor can expire

### Keyset Pagination

```
GET /items?after_id=123&limit=20
GET /items?created_after=2024-01-15T10:00:00Z&limit=20
```

**Pros**: Very efficient with proper indexes, consistent
**Cons**: Requires sortable unique field

### Recommendation

| Use Case | Strategy |
|----------|----------|
| Admin dashboards | Offset (page jumping needed) |
| Infinite scroll | Cursor (consistency matters) |
| Time-series data | Keyset (natural ordering) |
| Large datasets (>100k) | Cursor or Keyset |

---

## Versioning Strategies

### URL Path Versioning (Recommended)

```
https://api.example.com/v1/users
https://api.example.com/v2/users
```

**Pros**: Explicit, easy to route, cache-friendly
**Cons**: Encourages breaking changes

### Header Versioning

```
GET /users
Accept: application/vnd.example.v2+json
```

**Pros**: Clean URLs, content negotiation
**Cons**: Harder to test, less visible

### Query Parameter Versioning

```
GET /users?version=2
```

**Pros**: Easy to add, works with any client
**Cons**: Pollutes URL, optional params can be forgotten

### Version Migration Strategy

```markdown
## Deprecation Timeline

1. **Announce**: 6 months before deprecation
2. **Deprecation Headers**: Add `Deprecation` and `Sunset` headers
3. **Documentation**: Mark as deprecated, link to migration guide
4. **Usage Tracking**: Monitor v1 usage, reach out to heavy users
5. **Grace Period**: Maintain for 12 months after v2 stable
6. **Sunset**: Return 410 Gone with migration info
```

---

## Rate Limiting Design

### Response Headers

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 950
X-RateLimit-Reset: 1640995200
X-RateLimit-Policy: 1000;w=3600
Retry-After: 3600
```

### Rate Limit Strategies

| Strategy | Description | Best For |
|----------|-------------|----------|
| **Fixed Window** | Reset at fixed intervals | Simple, predictable |
| **Sliding Window** | Rolling time window | Smoother distribution |
| **Token Bucket** | Refill tokens over time | Burst tolerance |
| **Leaky Bucket** | Process at constant rate | Traffic shaping |

### Tiered Rate Limits

```yaml
rate_limits:
  anonymous:
    requests_per_hour: 60
    burst: 10
  authenticated:
    requests_per_hour: 5000
    burst: 100
  premium:
    requests_per_hour: 50000
    burst: 1000
  
  # Endpoint-specific
  endpoints:
    /search:
      requests_per_minute: 30
    /exports:
      requests_per_hour: 10
```

---

## GraphQL Design

### Schema Design Principles

```graphql
# Use descriptive types, not primitive obsession
type Money {
  amount: Int!  # In smallest currency unit (cents)
  currency: Currency!
}

type User {
  id: ID!
  email: EmailAddress!  # Custom scalar with validation
  profile: UserProfile!
  
  # Connections for pagination
  orders(
    first: Int
    after: String
    filter: OrderFilter
  ): OrderConnection!
  
  # Computed fields
  totalSpent: Money!
  memberSince: DateTime!
}

# Relay-style connections
type OrderConnection {
  edges: [OrderEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type OrderEdge {
  node: Order!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

### Query Design

```graphql
type Query {
  # Single resource by ID
  user(id: ID!): User
  
  # List with filtering, sorting, pagination
  users(
    filter: UserFilter
    orderBy: UserOrderBy
    first: Int
    after: String
  ): UserConnection!
  
  # Search across types
  search(query: String!, types: [SearchableType!]): SearchResults!
  
  # Viewer pattern for current user
  viewer: User
}

input UserFilter {
  status: UserStatus
  createdAfter: DateTime
  createdBefore: DateTime
  search: String
}

enum UserOrderBy {
  CREATED_AT_ASC
  CREATED_AT_DESC
  NAME_ASC
  NAME_DESC
}
```

### Mutation Design

```graphql
type Mutation {
  # Use input types
  createUser(input: CreateUserInput!): CreateUserPayload!
  updateUser(input: UpdateUserInput!): UpdateUserPayload!
  deleteUser(id: ID!): DeleteUserPayload!
  
  # Semantic mutations for complex operations
  inviteUserToOrganization(input: InviteUserInput!): InviteUserPayload!
  transferOwnership(input: TransferOwnershipInput!): TransferOwnershipPayload!
}

# Consistent payload pattern
type CreateUserPayload {
  user: User
  errors: [UserError!]!
}

type UserError {
  field: [String!]  # Path to the field with error
  code: ErrorCode!
  message: String!
}

input CreateUserInput {
  email: EmailAddress!
  name: String!
  organizationId: ID!
  role: UserRole = MEMBER
}
```

### N+1 Prevention

```javascript
// Use DataLoader pattern
const userLoader = new DataLoader(async (userIds) => {
  const users = await db.users.findMany({
    where: { id: { in: userIds } }
  });
  // Return in same order as requested IDs
  const userMap = new Map(users.map(u => [u.id, u]));
  return userIds.map(id => userMap.get(id));
});

// In resolver
const resolvers = {
  Order: {
    user: (order, args, { loaders }) => {
      return loaders.user.load(order.userId);
    }
  }
};
```

---

## Authentication & Authorization

### API Key Authentication

```http
# Header (recommended)
Authorization: Bearer api_key_xxxxx

# Query param (for webhooks, limited use)
GET /webhook?api_key=xxxxx
```

### OAuth 2.0 Flows

| Flow | Use Case |
|------|----------|
| Authorization Code + PKCE | Web apps, mobile apps |
| Client Credentials | Server-to-server |
| Device Code | CLI tools, TVs |

### JWT Structure

```json
{
  "header": {
    "alg": "RS256",
    "typ": "JWT",
    "kid": "key_2024_01"
  },
  "payload": {
    "sub": "user_123",
    "iss": "https://api.example.com",
    "aud": "https://api.example.com",
    "exp": 1640995200,
    "iat": 1640991600,
    "scope": "read:users write:orders",
    "org_id": "org_456"
  }
}
```

### Scopes Design

```yaml
scopes:
  # Resource-based
  read:users: "Read user profiles"
  write:users: "Create and update users"
  delete:users: "Delete users"
  
  # Admin scopes
  admin:read: "Read all resources"
  admin:write: "Modify all resources"
  
  # Feature-based
  billing: "Access billing information"
  reports: "Generate reports"
```

---

## OpenAPI Specification Best Practices

### Complete Example

```yaml
openapi: 3.1.0
info:
  title: Example API
  version: 1.0.0
  description: |
    ## Overview
    The Example API provides programmatic access to...
    
    ## Authentication
    All endpoints require Bearer token authentication.
    
    ## Rate Limiting
    See rate limit headers in responses.
  contact:
    name: API Support
    email: api@example.com
  license:
    name: MIT

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: https://api.staging.example.com/v1
    description: Staging

security:
  - bearerAuth: []

tags:
  - name: Users
    description: User management operations
  - name: Orders
    description: Order processing

paths:
  /users:
    get:
      operationId: listUsers
      summary: List all users
      description: |
        Retrieves a paginated list of users. Results can be filtered
        and sorted using query parameters.
      tags: [Users]
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/PerPageParam'
        - name: status
          in: query
          schema:
            $ref: '#/components/schemas/UserStatus'
        - name: sort
          in: query
          schema:
            type: string
            enum: [created_at, -created_at, name, -name]
            default: -created_at
      responses:
        '200':
          description: Successful response
          headers:
            X-RateLimit-Limit:
              $ref: '#/components/headers/X-RateLimit-Limit'
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserListResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '429':
          $ref: '#/components/responses/RateLimited'

    post:
      operationId: createUser
      summary: Create a user
      tags: [Users]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
            examples:
              basic:
                summary: Basic user creation
                value:
                  email: user@example.com
                  name: Jane Doe
      responses:
        '201':
          description: User created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '422':
          $ref: '#/components/responses/ValidationError'

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  schemas:
    User:
      type: object
      required: [id, email, name, created_at]
      properties:
        id:
          type: string
          pattern: '^user_[a-zA-Z0-9]+$'
          example: user_abc123
        email:
          type: string
          format: email
        name:
          type: string
          minLength: 1
          maxLength: 100
        status:
          $ref: '#/components/schemas/UserStatus'
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
          nullable: true

    UserStatus:
      type: string
      enum: [active, inactive, pending, suspended]

    Error:
      type: object
      required: [code, message]
      properties:
        code:
          type: string
        message:
          type: string
        details:
          type: array
          items:
            $ref: '#/components/schemas/ErrorDetail'
        request_id:
          type: string
        documentation_url:
          type: string
          format: uri

  parameters:
    PageParam:
      name: page
      in: query
      description: Page number (1-indexed)
      schema:
        type: integer
        minimum: 1
        default: 1

    PerPageParam:
      name: per_page
      in: query
      description: Items per page
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20

  responses:
    Unauthorized:
      description: Authentication required
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: unauthorized
            message: Authentication required

    RateLimited:
      description: Rate limit exceeded
      headers:
        Retry-After:
          schema:
            type: integer
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
```

---

## API Design Review Checklist

### Naming & Consistency

- [ ] Resource names are plural nouns (`/users`, not `/user`)
- [ ] URL paths use kebab-case (`/user-profiles`)
- [ ] Query params use snake_case (`created_after`)
- [ ] JSON fields use camelCase or snake_case consistently
- [ ] Consistent timestamp format (ISO 8601)
- [ ] Consistent ID format across resources

### Security

- [ ] HTTPS only (no HTTP)
- [ ] Authentication on all endpoints (explicit public marking)
- [ ] Authorization checks at resource level
- [ ] Rate limiting implemented
- [ ] No sensitive data in URLs
- [ ] Input validation on all fields
- [ ] Output encoding to prevent XSS

### Usability

- [ ] Consistent error format with actionable messages
- [ ] Pagination on all list endpoints
- [ ] Filtering and sorting where useful
- [ ] Appropriate HTTP status codes
- [ ] Versioning strategy defined
- [ ] CORS configured correctly
- [ ] Compression enabled (gzip, brotli)

### Documentation

- [ ] OpenAPI spec complete and accurate
- [ ] All endpoints documented
- [ ] Request/response examples provided
- [ ] Error codes documented
- [ ] Authentication explained
- [ ] Rate limits documented
- [ ] Changelog maintained

### Performance

- [ ] N+1 queries prevented
- [ ] Appropriate caching headers
- [ ] ETags for conditional requests
- [ ] Bulk operations for efficiency
- [ ] Async endpoints for long operations
- [ ] Response size limits

---

## Anti-Patterns to Avoid

### ❌ Verb in URLs

```
# Bad
POST /createUser
GET /getUserById/123
POST /users/123/delete

# Good
POST /users
GET /users/123
DELETE /users/123
```

### ❌ Inconsistent Plurality

```
# Bad
/user/123/orders
/users/123/order

# Good
/users/123/orders
```

### ❌ Exposing Internal IDs

```
# Bad - exposes database auto-increment
{ "id": 12345 }

# Good - opaque, prefixed identifiers
{ "id": "user_a1b2c3d4" }
```

### ❌ Breaking Changes Without Versioning

```
# Bad - field type change breaks clients
v1: { "price": "19.99" }
v2: { "price": 19.99 }

# Good - new field, deprecate old
v1: { "price": "19.99", "price_cents": 1999 }
```

### ❌ Overly Nested URLs

```
# Bad - too deep
/organizations/123/departments/456/teams/789/members/012/roles

# Good - flatten or use query params
/team-members/012?include=roles
/teams/789/members?include=roles
```

---

## Implementation Templates

### Express.js REST API

```typescript
// src/routes/users.ts
import { Router } from 'express';
import { z } from 'zod';
import { validate } from '../middleware/validate';
import { authenticate } from '../middleware/auth';
import { rateLimit } from '../middleware/rateLimit';
import { userService } from '../services/user';
import { ApiError } from '../utils/errors';

const router = Router();

const ListUsersQuery = z.object({
  page: z.coerce.number().int().min(1).default(1),
  per_page: z.coerce.number().int().min(1).max(100).default(20),
  status: z.enum(['active', 'inactive', 'pending']).optional(),
  sort: z.enum(['created_at', '-created_at', 'name', '-name']).default('-created_at'),
});

const CreateUserBody = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  role: z.enum(['admin', 'member', 'viewer']).default('member'),
});

router.get(
  '/',
  authenticate,
  rateLimit({ windowMs: 60000, max: 100 }),
  validate({ query: ListUsersQuery }),
  async (req, res, next) => {
    try {
      const { page, per_page, status, sort } = req.validatedQuery;
      
      const result = await userService.list({
        page,
        perPage: per_page,
        status,
        sort,
        organizationId: req.user.organizationId,
      });

      res.json({
        data: result.users.map(formatUser),
        meta: {
          total_count: result.total,
          page,
          per_page,
          total_pages: Math.ceil(result.total / per_page),
        },
        links: buildPaginationLinks(req, result.total, page, per_page),
      });
    } catch (error) {
      next(error);
    }
  }
);

router.post(
  '/',
  authenticate,
  rateLimit({ windowMs: 60000, max: 20 }),
  validate({ body: CreateUserBody }),
  async (req, res, next) => {
    try {
      const user = await userService.create({
        ...req.validatedBody,
        organizationId: req.user.organizationId,
        createdBy: req.user.id,
      });

      res.status(201).json({
        data: formatUser(user),
      });
    } catch (error) {
      if (error.code === 'DUPLICATE_EMAIL') {
        throw new ApiError(409, 'conflict', 'A user with this email already exists');
      }
      next(error);
    }
  }
);

export default router;
```

### FastAPI GraphQL

```python
# src/schema/user.py
import strawberry
from strawberry.types import Info
from typing import Optional, List
from dataclasses import dataclass

from src.dataloaders import get_loaders
from src.services.user import UserService
from src.models import UserModel

@strawberry.type
class User:
    id: strawberry.ID
    email: str
    name: str
    status: str
    created_at: str
    
    @strawberry.field
    async def organization(self, info: Info) -> "Organization":
        loaders = get_loaders(info.context)
        return await loaders.organization.load(self.organization_id)
    
    @strawberry.field
    async def orders(
        self,
        info: Info,
        first: Optional[int] = 20,
        after: Optional[str] = None,
    ) -> "OrderConnection":
        service = info.context.order_service
        return await service.list_by_user(
            user_id=self.id,
            first=first,
            after=after,
        )

@strawberry.input
class CreateUserInput:
    email: str
    name: str
    organization_id: strawberry.ID
    role: str = "member"

@strawberry.type
class CreateUserPayload:
    user: Optional[User] = None
    errors: List["UserError"] = strawberry.field(default_factory=list)

@strawberry.type
class UserError:
    field: List[str]
    code: str
    message: str

@strawberry.type
class Query:
    @strawberry.field
    async def user(self, info: Info, id: strawberry.ID) -> Optional[User]:
        service: UserService = info.context.user_service
        return await service.get_by_id(id)
    
    @strawberry.field
    async def users(
        self,
        info: Info,
        first: Optional[int] = 20,
        after: Optional[str] = None,
        filter: Optional["UserFilter"] = None,
    ) -> "UserConnection":
        service: UserService = info.context.user_service
        return await service.list(first=first, after=after, filter=filter)

@strawberry.type
class Mutation:
    @strawberry.mutation
    async def create_user(
        self,
        info: Info,
        input: CreateUserInput,
    ) -> CreateUserPayload:
        service: UserService = info.context.user_service
        
        try:
            user = await service.create(
                email=input.email,
                name=input.name,
                organization_id=input.organization_id,
                role=input.role,
            )
            return CreateUserPayload(user=user)
        except ValidationError as e:
            return CreateUserPayload(errors=[
                UserError(field=e.field, code=e.code, message=e.message)
            ])
```

---

## Final Step: Quality Assurance

Before delivering any API design:

1. **Schema Validation**: Validate OpenAPI spec with `swagger-cli validate`
2. **Mock Server**: Generate mock server to test consumer integration
3. **Documentation Review**: Ensure all endpoints have clear descriptions and examples
4. **Security Audit**: Run through security checklist
5. **Performance Analysis**: Estimate response sizes and query complexity
6. **Developer Testing**: Have someone unfamiliar with the API try the docs

The API is ready when a developer can successfully make their first API call within 5 minutes of reading the documentation.
