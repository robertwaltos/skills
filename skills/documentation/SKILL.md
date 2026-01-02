---
name: documentation
description: Expert-level technical documentation - API docs, architecture documentation, user guides, runbooks, and documentation-as-code practices
keywords: [documentation, technical-writing, api-docs, readme, architecture, runbooks, markdown, openapi, swagger, docusaurus, mkdocs, sphinx]
use_cases:
  - API documentation (OpenAPI, GraphQL schemas)
  - README files and project documentation
  - Architecture Decision Records (ADRs)
  - System design documentation
  - User guides and tutorials
  - Runbooks and operational documentation
  - Code documentation and comments
  - Documentation sites (Docusaurus, MkDocs)
license: MIT
---

# Documentation Skill

> **Expertise Level**: Principal Technical Writer / Staff Engineer - Equivalent to 15+ years documentation and technical communication experience

## Purpose

Transform complex technical concepts into clear, actionable documentation. This skill provides expert-level guidance for creating comprehensive, maintainable documentation that serves developers, operators, and end users—covering everything from inline code comments to full documentation sites.

## Related Skills

- **api-design** - API documentation best practices
- **devops-automation** - Runbook and operational documentation
- **code-review** - Documentation review practices
- **user-research** - Understanding documentation audiences

---

## Core Process

### Phase 1: Documentation Assessment

```markdown
## Documentation Audit

### Current State
| Document Type | Exists | Quality (1-5) | Up-to-date | Location |
|--------------|--------|---------------|------------|----------|
| README | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Path] |
| API Reference | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Path] |
| Architecture docs | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Path] |
| Getting Started | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Path] |
| Runbooks | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Path] |
| Code comments | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Inline] |
| CHANGELOG | [Yes/No] | [⭐-⭐⭐⭐⭐⭐] | [Yes/No] | [Path] |

### Audience Analysis
| Audience | Primary Needs | Current Gaps |
|----------|---------------|--------------|
| New developers | [Onboarding, setup] | [List gaps] |
| API consumers | [Integration guides] | [List gaps] |
| Operators | [Deployment, monitoring] | [List gaps] |
| End users | [Features, workflows] | [List gaps] |
```

### Phase 2: README Excellence

```markdown
# Project Name

> One-line description that immediately conveys the project's purpose.

[![Build Status](https://img.shields.io/github/actions/workflow/status/org/repo/ci.yml?branch=main)](https://github.com/org/repo/actions)
[![npm version](https://img.shields.io/npm/v/package-name)](https://www.npmjs.com/package/package-name)
[![License](https://img.shields.io/github/license/org/repo)](LICENSE)

## Why This Project?

Brief explanation of the problem this solves and why someone should use it.
Include a key differentiator or unique value proposition.

## Features

- **Feature 1**: Brief description of what it does
- **Feature 2**: Brief description of what it does
- **Feature 3**: Brief description of what it does

## Quick Start

### Prerequisites

- Node.js >= 18.0.0
- npm >= 9.0.0

### Installation

```bash
npm install package-name
```

### Basic Usage

```typescript
import { Client } from 'package-name';

const client = new Client({ apiKey: 'your-api-key' });
const result = await client.doSomething();
console.log(result);
```

## Documentation

- [Full Documentation](https://docs.example.com)
- [API Reference](https://docs.example.com/api)
- [Examples](./examples)
- [CHANGELOG](./CHANGELOG.md)

## Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `apiKey` | `string` | required | Your API key |
| `timeout` | `number` | `30000` | Request timeout in ms |
| `retries` | `number` | `3` | Number of retry attempts |

## Examples

See the [examples](./examples) directory for:

- [Basic Usage](./examples/basic.ts)
- [Advanced Configuration](./examples/advanced.ts)
- [Error Handling](./examples/error-handling.ts)

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md).

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

[MIT](LICENSE) © [Your Name]

## Support

- 📚 [Documentation](https://docs.example.com)
- 💬 [Discussions](https://github.com/org/repo/discussions)
- 🐛 [Issues](https://github.com/org/repo/issues)
```

### Phase 3: API Documentation (OpenAPI)

```yaml
openapi: 3.1.0
info:
  title: User Management API
  description: |
    ## Overview
    
    The User Management API provides comprehensive functionality for managing
    user accounts, authentication, and permissions.
    
    ## Authentication
    
    All API requests require authentication via Bearer token:
    
    ```bash
    curl -H "Authorization: Bearer <token>" https://api.example.com/users
    ```
    
    ## Rate Limiting
    
    - **Standard tier**: 1000 requests per minute
    - **Enterprise tier**: 10000 requests per minute
    
    Rate limit headers are included in all responses:
    - `X-RateLimit-Limit`: Maximum requests per window
    - `X-RateLimit-Remaining`: Remaining requests
    - `X-RateLimit-Reset`: Unix timestamp when limit resets
    
    ## Error Handling
    
    All errors follow RFC 7807 (Problem Details for HTTP APIs):
    
    ```json
    {
      "type": "https://api.example.com/errors/validation-error",
      "title": "Validation Error",
      "status": 400,
      "detail": "The email field is invalid",
      "instance": "/users/123"
    }
    ```
  version: 2.0.0
  contact:
    name: API Support
    email: api-support@example.com
    url: https://docs.example.com/support
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.example.com/v2
    description: Production server
  - url: https://staging-api.example.com/v2
    description: Staging server
  - url: http://localhost:3000/v2
    description: Local development

tags:
  - name: Users
    description: User account management
  - name: Authentication
    description: Authentication and authorization

paths:
  /users:
    get:
      tags: [Users]
      operationId: listUsers
      summary: List all users
      description: |
        Retrieves a paginated list of users. Results can be filtered
        by status, role, and other attributes.
        
        ### Pagination
        
        Use `page` and `limit` parameters for pagination.
        Response includes `X-Total-Count` header with total results.
        
        ### Example
        
        ```bash
        curl -H "Authorization: Bearer <token>" \
          "https://api.example.com/v2/users?status=active&limit=10"
        ```
      parameters:
        - name: status
          in: query
          description: Filter by user status
          schema:
            type: string
            enum: [active, inactive, pending]
          example: active
        - name: role
          in: query
          description: Filter by user role
          schema:
            type: string
            enum: [admin, user, viewer]
        - name: page
          in: query
          description: Page number (1-indexed)
          schema:
            type: integer
            minimum: 1
            default: 1
        - name: limit
          in: query
          description: Results per page
          schema:
            type: integer
            minimum: 1
            maximum: 100
            default: 20
      responses:
        '200':
          description: Successful response
          headers:
            X-Total-Count:
              description: Total number of results
              schema:
                type: integer
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
              example:
                - id: "usr_123abc"
                  email: "john@example.com"
                  name: "John Doe"
                  status: active
                  createdAt: "2024-01-15T10:30:00Z"
        '401':
          $ref: '#/components/responses/Unauthorized'
        '429':
          $ref: '#/components/responses/RateLimited'

    post:
      tags: [Users]
      operationId: createUser
      summary: Create a new user
      description: |
        Creates a new user account. A verification email will be sent
        to the provided email address.
        
        ### Required Fields
        
        - `email`: Valid email address (unique)
        - `name`: User's display name
        
        ### Example
        
        ```bash
        curl -X POST \
          -H "Authorization: Bearer <token>" \
          -H "Content-Type: application/json" \
          -d '{"email": "new@example.com", "name": "New User"}' \
          https://api.example.com/v2/users
        ```
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
            example:
              email: "new@example.com"
              name: "New User"
              role: user
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/ValidationError'
        '409':
          description: User with this email already exists
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

components:
  schemas:
    User:
      type: object
      description: Represents a user account
      properties:
        id:
          type: string
          description: Unique user identifier
          pattern: ^usr_[a-zA-Z0-9]+$
          example: usr_123abc
        email:
          type: string
          format: email
          description: User's email address
          example: john@example.com
        name:
          type: string
          description: User's display name
          minLength: 1
          maxLength: 100
          example: John Doe
        status:
          type: string
          enum: [active, inactive, pending]
          description: Current account status
          example: active
        role:
          type: string
          enum: [admin, user, viewer]
          description: User's role
          example: user
        createdAt:
          type: string
          format: date-time
          description: Account creation timestamp
          example: "2024-01-15T10:30:00Z"
        updatedAt:
          type: string
          format: date-time
          description: Last update timestamp
          example: "2024-01-20T14:45:00Z"
      required:
        - id
        - email
        - name
        - status
        - createdAt

    CreateUserRequest:
      type: object
      properties:
        email:
          type: string
          format: email
          description: User's email address (must be unique)
        name:
          type: string
          minLength: 1
          maxLength: 100
          description: User's display name
        role:
          type: string
          enum: [admin, user, viewer]
          default: user
          description: Initial role assignment
      required:
        - email
        - name

    Error:
      type: object
      description: RFC 7807 compliant error response
      properties:
        type:
          type: string
          format: uri
          description: Error type URI
        title:
          type: string
          description: Human-readable error title
        status:
          type: integer
          description: HTTP status code
        detail:
          type: string
          description: Detailed error description
        instance:
          type: string
          description: URI reference to the specific occurrence

  responses:
    Unauthorized:
      description: Authentication required or invalid
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            type: https://api.example.com/errors/unauthorized
            title: Unauthorized
            status: 401
            detail: Invalid or expired authentication token

    ValidationError:
      description: Request validation failed
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    RateLimited:
      description: Rate limit exceeded
      headers:
        Retry-After:
          description: Seconds until rate limit resets
          schema:
            type: integer
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: |
        JWT token obtained from the `/auth/token` endpoint.
        Include in the `Authorization` header as `Bearer <token>`.

security:
  - bearerAuth: []
```

### Phase 4: Architecture Decision Records

```markdown
# ADR-001: Adopt Event-Driven Architecture

## Status

**Accepted** - 2024-01-15

## Context

Our monolithic application is experiencing scaling challenges:

- Database contention during peak hours
- Tight coupling between services making changes risky
- Difficulty scaling individual components independently
- Long deployment times affecting velocity

We need an architecture that allows independent scaling and deployment.

## Decision

We will adopt an event-driven architecture using:

- **Apache Kafka** as the event streaming platform
- **Event sourcing** for core domain entities
- **CQRS** pattern for read-heavy operations
- **Saga pattern** for distributed transactions

### Architecture Diagram

```
┌──────────────┐    Events    ┌─────────────────┐
│   Service A  │─────────────▶│      Kafka      │
└──────────────┘              │   (Event Bus)   │
                              └────────┬────────┘
                                       │
         ┌─────────────────────────────┼─────────────────────────────┐
         │                             │                             │
         ▼                             ▼                             ▼
┌──────────────┐             ┌──────────────┐             ┌──────────────┐
│   Service B  │             │   Service C  │             │  Analytics   │
│  (Consumer)  │             │  (Consumer)  │             │  (Consumer)  │
└──────────────┘             └──────────────┘             └──────────────┘
```

## Consequences

### Positive

- **Independent scaling**: Services can scale based on their specific load
- **Loose coupling**: Services communicate via events, reducing direct dependencies
- **Resilience**: Failures in one service don't cascade to others
- **Audit trail**: Event log provides complete history of system state changes
- **Flexibility**: New consumers can be added without modifying producers

### Negative

- **Increased complexity**: Distributed systems are harder to debug
- **Eventual consistency**: Data may not be immediately consistent across services
- **Learning curve**: Team needs to learn event-driven patterns
- **Infrastructure overhead**: Kafka cluster requires operational expertise

### Neutral

- **Testing changes**: Integration tests need to account for async behavior
- **Monitoring requirements**: Need distributed tracing (Jaeger/Zipkin)

## Alternatives Considered

### 1. Microservices with REST
- **Pros**: Simpler, well-understood pattern
- **Cons**: Synchronous calls create coupling, harder to scale

### 2. GraphQL Federation
- **Pros**: Single endpoint, efficient querying
- **Cons**: Doesn't solve scaling issues, adds complexity

### 3. Database per Service (without events)
- **Pros**: Service isolation
- **Cons**: Data synchronization becomes complex

## References

- [Martin Fowler - Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Microsoft - CQRS Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)
- [Kafka Documentation](https://kafka.apache.org/documentation/)

## Changelog

| Date | Author | Change |
|------|--------|--------|
| 2024-01-15 | @techarch | Initial decision |
| 2024-02-01 | @devlead | Added implementation timeline |
```

### Phase 5: Runbook Documentation

```markdown
# Runbook: API Service Incident Response

## Service Overview

| Property | Value |
|----------|-------|
| Service | api-service |
| Team | Platform |
| On-call | #platform-oncall |
| Dashboards | [Grafana](https://grafana.example.com/d/api-service) |
| Logs | [Kibana](https://kibana.example.com/app/logs?service=api-service) |
| Repository | [github.com/org/api-service](https://github.com/org/api-service) |

---

## Alert: High Error Rate

### Trigger Condition
Error rate > 5% for 5 minutes

### Severity
**P1 - Critical** if affecting production traffic

### Symptoms
- Elevated 5xx responses in load balancer
- Increased error count in application logs
- User reports of failures

### Diagnostic Steps

1. **Check current error rate**
   ```bash
   kubectl exec -it deploy/api-service -- curl localhost:8080/metrics | grep http_errors
   ```

2. **Review recent logs**
   ```bash
   kubectl logs -l app=api-service --since=10m | grep -i error | tail -100
   ```

3. **Check database connectivity**
   ```bash
   kubectl exec -it deploy/api-service -- nc -zv postgres.db.svc.cluster.local 5432
   ```

4. **Check external dependencies**
   ```bash
   kubectl exec -it deploy/api-service -- curl -s https://auth.example.com/health
   ```

5. **Review recent deployments**
   ```bash
   kubectl rollout history deployment/api-service
   ```

### Resolution Steps

#### If database connection issues:
1. Check database status in AWS RDS console
2. Verify security group rules haven't changed
3. Check if connection pool is exhausted:
   ```bash
   kubectl exec -it deploy/api-service -- curl localhost:8080/actuator/metrics/hikaricp.connections
   ```
4. Scale up connection pool if needed (update ConfigMap)

#### If external dependency failure:
1. Enable circuit breaker if not already active
2. Contact dependent service team
3. Consider activating degraded mode

#### If recent deployment caused issue:
1. **Immediate rollback**:
   ```bash
   kubectl rollout undo deployment/api-service
   ```
2. Verify rollback success:
   ```bash
   kubectl rollout status deployment/api-service
   ```
3. Monitor error rate returning to normal

### Escalation

| Time | Action |
|------|--------|
| 0-15 min | On-call engineer investigates |
| 15-30 min | Escalate to secondary on-call |
| 30+ min | Page engineering manager |

### Post-Incident

- [ ] Create incident ticket
- [ ] Update status page
- [ ] Schedule post-mortem within 48 hours
- [ ] Update this runbook if procedures changed

---

## Alert: High Latency

### Trigger Condition
P99 latency > 2 seconds for 5 minutes

### Diagnostic Steps

1. **Identify slow endpoints**
   ```bash
   kubectl exec -it deploy/api-service -- \
     curl localhost:8080/actuator/metrics/http.server.requests | jq '.measurements'
   ```

2. **Check CPU/Memory pressure**
   ```bash
   kubectl top pods -l app=api-service
   ```

3. **Review database query performance**
   - Check slow query log in RDS
   - Review connection pool metrics

4. **Check for resource contention**
   ```bash
   kubectl describe node $(kubectl get pods -l app=api-service -o jsonpath='{.items[0].spec.nodeName}')
   ```

### Resolution Steps

1. **Scale horizontally if CPU-bound**:
   ```bash
   kubectl scale deployment/api-service --replicas=10
   ```

2. **Enable query caching if database-bound**:
   ```bash
   kubectl set env deployment/api-service CACHE_ENABLED=true
   ```

3. **Increase memory if OOM risk**:
   ```bash
   kubectl set resources deployment/api-service -c api --limits=memory=2Gi
   ```
```

---

## Expert Decision Frameworks

### Documentation Type Selection

| Documentation Type | Audience | Update Frequency | Location |
|-------------------|----------|------------------|----------|
| **README** | Everyone | With releases | Repository root |
| **API Reference** | Developers | With API changes | Generated from code |
| **Architecture docs** | Engineers | Major changes | docs/ or wiki |
| **Tutorials** | New users | Quarterly | Documentation site |
| **Runbooks** | Operations | After incidents | Internal wiki |
| **ADRs** | Engineers | Per decision | docs/adr/ |
| **CHANGELOG** | All users | Every release | Repository root |

### Documentation Tool Selection

| Tool | Best For | Pros | Cons |
|------|----------|------|------|
| **Docusaurus** | Developer docs | React-based, versioning | Complex setup |
| **MkDocs** | Technical docs | Simple, fast | Limited theming |
| **Sphinx** | Python projects | Auto-generation | Steep learning curve |
| **GitBook** | User guides | Beautiful UI | Vendor lock-in |
| **Notion** | Internal docs | Collaborative | Export limitations |

---

## Anti-Patterns to Avoid

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| **No README** | New developers lost | Always have a README |
| **Outdated docs** | Misleading information | Docs as code, CI checks |
| **Wall of text** | Not scannable | Use headings, lists, tables |
| **Missing examples** | Hard to understand | Include working code samples |
| **Jargon overload** | Excludes newcomers | Define terms, link glossary |
| **No search** | Can't find information | Use searchable doc platform |
| **Buried in code** | Documentation is invisible | Separate docs directory |

---

## Quick Reference

```bash
# Generate OpenAPI docs
npx @redocly/cli build-docs openapi.yaml -o docs/api.html

# Lint documentation
npx markdownlint-cli2 "docs/**/*.md"

# Spell check
npx cspell "docs/**/*.md"

# Check links
npx linkinator docs/ --recurse

# Start Docusaurus dev
npx docusaurus start

# Build MkDocs
mkdocs build

# Generate TypeDoc
npx typedoc --out docs/api src/index.ts
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-01-13 | Initial expert skill creation |
