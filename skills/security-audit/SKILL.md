---
name: security-audit
description: |
  Conduct comprehensive security audits, identify vulnerabilities, and implement robust security controls. Embodies expertise of a Principal Security Engineer with 15+ years in application security, penetration testing, and security architecture.
  Use when: user requests security review, vulnerability assessment, threat modeling, compliance checks, or security hardening.
  Outputs: Security audit reports, threat models, vulnerability assessments, remediation plans, and security implementation guides.
  Keywords: security, vulnerabilities, OWASP, penetration testing, threat modeling, authentication, authorization, encryption, XSS, SQL injection, CSRF, compliance, SOC2, GDPR
license: Complete terms in LICENSE.txt
---

# Security Audit Skill

**Expertise Level**: Principal Security Engineer (15+ years equivalent)

This skill embodies the methodology of elite security teams at companies like Google Project Zero, Trail of Bits, and Cure53—systematically identifying and eliminating security vulnerabilities before attackers can exploit them.

---

## Related Skills

- **[api-design](../api-design)**: Design secure APIs from the ground up
- **[database-design](../database-design)**: Implement secure data storage patterns
- **[devops-automation](../devops-automation)**: Automate security scanning in CI/CD
- **[code-review](../code-review)**: Include security in code review process

---

## Core Philosophy

**Security is not a feature—it's a property of the system.** Every security decision should:

1. **Defense in Depth**: Multiple layers, no single point of failure
2. **Least Privilege**: Grant minimum necessary access
3. **Fail Secure**: Deny by default, fail closed
4. **Zero Trust**: Verify explicitly, assume breach
5. **Security by Design**: Build security in, don't bolt it on

---

## Security Audit Process

### Phase 1: Reconnaissance & Scoping

```markdown
## Audit Scope Definition

### System Overview
- [ ] Application architecture diagram
- [ ] Data flow diagrams
- [ ] Technology stack inventory
- [ ] Third-party dependencies list
- [ ] Deployment environment details

### Assets to Protect
- [ ] User data (PII, credentials)
- [ ] Business-critical data
- [ ] API keys and secrets
- [ ] Intellectual property
- [ ] System availability

### Threat Actors
- [ ] External attackers (opportunistic, targeted)
- [ ] Malicious insiders
- [ ] Compromised third parties
- [ ] Nation-state actors (if applicable)

### Compliance Requirements
- [ ] GDPR / CCPA
- [ ] SOC 2
- [ ] PCI-DSS
- [ ] HIPAA
- [ ] Industry-specific regulations
```

### Phase 2: Threat Modeling (STRIDE)

```markdown
## STRIDE Analysis Template

### 1. Spoofing (Authentication)
**Question**: Can an attacker pretend to be someone else?

| Asset | Threat | Mitigation | Status |
|-------|--------|------------|--------|
| User login | Credential stuffing | Rate limiting, MFA, breach detection | ⚠️ |
| API endpoints | Token theft | Short expiry, rotation, binding | ✅ |
| Email | Phishing | SPF/DKIM/DMARC, user training | ⚠️ |

### 2. Tampering (Integrity)
**Question**: Can an attacker modify data they shouldn't?

| Asset | Threat | Mitigation | Status |
|-------|--------|------------|--------|
| Database | SQL injection | Parameterized queries, WAF | ✅ |
| File uploads | Malicious files | Type validation, sandboxing | ⚠️ |
| Session data | Cookie tampering | Signed cookies, server-side sessions | ✅ |

### 3. Repudiation (Non-repudiation)
**Question**: Can users deny performing actions?

| Asset | Threat | Mitigation | Status |
|-------|--------|------------|--------|
| Transactions | Action denial | Audit logs, digital signatures | ⚠️ |
| Admin actions | Unauthorized changes | Immutable audit trail | ❌ |

### 4. Information Disclosure (Confidentiality)
**Question**: Can attackers access data they shouldn't?

| Asset | Threat | Mitigation | Status |
|-------|--------|------------|--------|
| User data | Data breach | Encryption at rest/transit | ✅ |
| Error messages | Information leakage | Generic errors, logging | ⚠️ |
| Source code | Repository leak | Access controls, secrets scanning | ✅ |

### 5. Denial of Service (Availability)
**Question**: Can attackers make the system unavailable?

| Asset | Threat | Mitigation | Status |
|-------|--------|------------|--------|
| API | Rate exhaustion | Rate limiting, CDN | ✅ |
| Database | Resource exhaustion | Query limits, connection pools | ⚠️ |
| Login | Account lockout | Progressive delays, CAPTCHA | ✅ |

### 6. Elevation of Privilege (Authorization)
**Question**: Can users gain unauthorized access?

| Asset | Threat | Mitigation | Status |
|-------|--------|------------|--------|
| Admin functions | Privilege escalation | RBAC, permission checks | ⚠️ |
| Multi-tenant | Tenant isolation | Row-level security, testing | ✅ |
| File access | Path traversal | Canonicalization, sandboxing | ✅ |
```

### Phase 3: Vulnerability Assessment

---

## OWASP Top 10 Analysis

### A01:2021 - Broken Access Control

**Vulnerabilities to Check:**

```python
# ❌ VULNERABLE: Direct object reference without authorization
@app.get("/api/users/{user_id}/documents/{doc_id}")
async def get_document(user_id: int, doc_id: int):
    return await db.get_document(doc_id)  # No ownership check!

# ✅ SECURE: Verify ownership before returning
@app.get("/api/users/{user_id}/documents/{doc_id}")
async def get_document(user_id: int, doc_id: int, current_user: User = Depends(get_current_user)):
    if current_user.id != user_id and not current_user.is_admin:
        raise HTTPException(status_code=403, detail="Forbidden")
    
    document = await db.get_document(doc_id)
    if document.owner_id != user_id:
        raise HTTPException(status_code=404, detail="Not found")
    
    return document
```

**Checklist:**
- [ ] IDOR (Insecure Direct Object References) on all endpoints
- [ ] Horizontal privilege escalation (accessing other users' data)
- [ ] Vertical privilege escalation (admin functions)
- [ ] Missing function-level access control
- [ ] Metadata manipulation (JWT claims, hidden fields)
- [ ] CORS misconfiguration

### A02:2021 - Cryptographic Failures

**Vulnerabilities to Check:**

```python
# ❌ VULNERABLE: Weak hashing
import hashlib
password_hash = hashlib.md5(password.encode()).hexdigest()

# ✅ SECURE: Strong password hashing
from argon2 import PasswordHasher
ph = PasswordHasher(
    time_cost=3,
    memory_cost=65536,
    parallelism=4
)
password_hash = ph.hash(password)

# ❌ VULNERABLE: ECB mode
from Crypto.Cipher import AES
cipher = AES.new(key, AES.MODE_ECB)

# ✅ SECURE: GCM mode with authentication
cipher = AES.new(key, AES.MODE_GCM, nonce=nonce)
ciphertext, tag = cipher.encrypt_and_digest(plaintext)
```

**Checklist:**
- [ ] TLS 1.2+ enforced everywhere
- [ ] Strong cipher suites only
- [ ] Passwords hashed with Argon2id/bcrypt/scrypt
- [ ] Encryption at rest for sensitive data
- [ ] No hardcoded secrets in code
- [ ] Proper key management (rotation, storage)
- [ ] No sensitive data in URLs or logs

### A03:2021 - Injection

**Vulnerabilities to Check:**

```python
# ❌ VULNERABLE: SQL Injection
query = f"SELECT * FROM users WHERE email = '{email}'"
cursor.execute(query)

# ✅ SECURE: Parameterized query
cursor.execute("SELECT * FROM users WHERE email = %s", (email,))

# ❌ VULNERABLE: Command Injection
os.system(f"convert {filename} output.png")

# ✅ SECURE: Use subprocess with array
import subprocess
subprocess.run(["convert", filename, "output.png"], check=True)

# ❌ VULNERABLE: NoSQL Injection (MongoDB)
db.users.find({"$where": f"this.email == '{email}'"})

# ✅ SECURE: Use query operators
db.users.find({"email": email})

# ❌ VULNERABLE: LDAP Injection
ldap_filter = f"(uid={username})"

# ✅ SECURE: Escape special characters
from ldap3.utils.conv import escape_filter_chars
ldap_filter = f"(uid={escape_filter_chars(username)})"
```

**Checklist:**
- [ ] All SQL queries parameterized
- [ ] ORM with parameterized queries
- [ ] OS command execution avoided or sanitized
- [ ] XML parsing with external entities disabled
- [ ] Template engines with auto-escaping
- [ ] LDAP queries escaped

### A04:2021 - Insecure Design

**Architecture Review:**

```markdown
## Secure Design Principles

### Authentication
- [ ] Multi-factor authentication available
- [ ] Account lockout with exponential backoff
- [ ] Secure password reset flow
- [ ] Session management (timeout, invalidation)

### Authorization
- [ ] Role-based access control (RBAC)
- [ ] Principle of least privilege
- [ ] Permission checks at every layer
- [ ] Audit logging for sensitive actions

### Data Protection
- [ ] Data classification scheme
- [ ] Encryption strategy defined
- [ ] Data retention policies
- [ ] Secure deletion procedures

### Third-Party Integration
- [ ] Vendor security assessment
- [ ] Minimal data sharing
- [ ] API key rotation capability
- [ ] Webhook signature verification
```

### A05:2021 - Security Misconfiguration

**Configuration Audit:**

```yaml
# Security Headers (nginx example)
add_header X-Frame-Options "DENY" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';" always;
add_header Permissions-Policy "geolocation=(), microphone=(), camera=()" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;

# Disable server version disclosure
server_tokens off;
```

**Checklist:**
- [ ] Security headers configured
- [ ] Debug mode disabled in production
- [ ] Default credentials changed
- [ ] Unnecessary features/ports disabled
- [ ] Error messages don't leak information
- [ ] Directory listing disabled
- [ ] Cloud storage permissions verified
- [ ] Container security (non-root, read-only)

### A06:2021 - Vulnerable Components

**Dependency Audit:**

```bash
# Python
pip-audit
safety check

# Node.js
npm audit
snyk test

# Ruby
bundle audit

# Go
govulncheck ./...

# Docker
trivy image myapp:latest
grype myapp:latest
```

**Checklist:**
- [ ] Dependency scanning in CI/CD
- [ ] Automated updates (Dependabot, Renovate)
- [ ] SBOM (Software Bill of Materials) generated
- [ ] Known vulnerability monitoring
- [ ] Container base image updates
- [ ] License compliance check

### A07:2021 - Authentication Failures

**Authentication Security:**

```python
# Secure password requirements
PASSWORD_REQUIREMENTS = {
    "min_length": 12,
    "require_uppercase": True,
    "require_lowercase": True,
    "require_digit": True,
    "require_special": True,
    "check_common_passwords": True,
    "check_breached_passwords": True,  # HaveIBeenPwned API
}

# Secure session configuration
SESSION_CONFIG = {
    "cookie_secure": True,
    "cookie_httponly": True,
    "cookie_samesite": "Strict",
    "session_timeout": 3600,  # 1 hour
    "absolute_timeout": 86400,  # 24 hours
    "regenerate_on_auth": True,
}

# Rate limiting for authentication
RATE_LIMITS = {
    "login_attempts": "5/minute",
    "password_reset": "3/hour",
    "mfa_attempts": "5/minute",
}
```

**Checklist:**
- [ ] Strong password policy enforced
- [ ] Multi-factor authentication available
- [ ] Secure session management
- [ ] Brute force protection
- [ ] Credential stuffing protection
- [ ] Secure password reset (no user enumeration)
- [ ] Session invalidation on logout/password change

### A08:2021 - Software and Data Integrity

**Supply Chain Security:**

```yaml
# GitHub Actions - Pin dependencies
- uses: actions/checkout@v4.1.1  # Pin to specific version
  with:
    persist-credentials: false

# Verify signatures
- name: Verify GPG signature
  run: |
    gpg --verify release.sig release.tar.gz
    
# Lock file verification
- name: Check lockfile
  run: |
    npm ci --ignore-scripts  # Use lockfile, no scripts
```

**Checklist:**
- [ ] CI/CD pipeline secured
- [ ] Dependency lockfiles committed
- [ ] Code signing implemented
- [ ] Artifact integrity verification
- [ ] Auto-update mechanisms secured
- [ ] Webhook signatures verified

### A09:2021 - Security Logging and Monitoring

**Logging Configuration:**

```python
import structlog
from datetime import datetime

logger = structlog.get_logger()

# Security event logging
def log_security_event(event_type: str, details: dict, severity: str = "medium"):
    logger.info(
        "security_event",
        event_type=event_type,
        severity=severity,
        timestamp=datetime.utcnow().isoformat(),
        **details
    )

# Events to log
SECURITY_EVENTS = [
    "login_success",
    "login_failure",
    "logout",
    "password_change",
    "mfa_enabled",
    "mfa_disabled",
    "permission_denied",
    "rate_limit_exceeded",
    "suspicious_activity",
    "data_export",
    "admin_action",
]
```

**Checklist:**
- [ ] Authentication events logged
- [ ] Authorization failures logged
- [ ] Input validation failures logged
- [ ] Sensitive operations logged
- [ ] Logs don't contain sensitive data
- [ ] Log integrity protection
- [ ] Alerting on anomalies
- [ ] Log retention policy

### A10:2021 - Server-Side Request Forgery (SSRF)

**SSRF Prevention:**

```python
import ipaddress
from urllib.parse import urlparse

BLOCKED_HOSTS = {
    "localhost",
    "127.0.0.1",
    "0.0.0.0",
    "169.254.169.254",  # AWS metadata
    "metadata.google.internal",  # GCP metadata
}

def is_safe_url(url: str) -> bool:
    """Validate URL is safe for server-side requests."""
    try:
        parsed = urlparse(url)
        
        # Only allow HTTP/HTTPS
        if parsed.scheme not in ("http", "https"):
            return False
        
        # Block known dangerous hosts
        if parsed.hostname in BLOCKED_HOSTS:
            return False
        
        # Block private IP ranges
        try:
            ip = ipaddress.ip_address(parsed.hostname)
            if ip.is_private or ip.is_loopback or ip.is_link_local:
                return False
        except ValueError:
            pass  # Not an IP, continue
        
        return True
    except Exception:
        return False

# ❌ VULNERABLE
response = requests.get(user_provided_url)

# ✅ SECURE
if not is_safe_url(user_provided_url):
    raise ValueError("Invalid URL")
response = requests.get(user_provided_url, timeout=10)
```

---

## Web Application Security

### XSS Prevention

```javascript
// ❌ VULNERABLE: innerHTML with user data
element.innerHTML = userInput;

// ✅ SECURE: textContent for text
element.textContent = userInput;

// ✅ SECURE: DOMPurify for HTML
import DOMPurify from 'dompurify';
element.innerHTML = DOMPurify.sanitize(userHtml);

// React - automatic escaping
// ❌ VULNERABLE
<div dangerouslySetInnerHTML={{__html: userInput}} />

// ✅ SECURE - React escapes by default
<div>{userInput}</div>
```

### CSRF Prevention

```python
# Django - CSRF token in forms
{% csrf_token %}

# SPA - CSRF token in headers
from django.middleware.csrf import get_token

def get_csrf_token(request):
    return JsonResponse({"csrfToken": get_token(request)})

# Client-side
fetch('/api/submit', {
    method: 'POST',
    headers: {
        'X-CSRFToken': csrfToken,
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(data),
});
```

### Content Security Policy

```http
# Strict CSP
Content-Security-Policy: 
  default-src 'self';
  script-src 'self' 'nonce-{random}';
  style-src 'self' 'nonce-{random}';
  img-src 'self' data: https:;
  font-src 'self';
  connect-src 'self' https://api.example.com;
  frame-ancestors 'none';
  form-action 'self';
  base-uri 'self';
  upgrade-insecure-requests;

# Report violations
Content-Security-Policy-Report-Only: ...
  report-uri /csp-report;
```

---

## API Security

### Authentication Best Practices

```python
# JWT Configuration
JWT_CONFIG = {
    "algorithm": "RS256",  # Asymmetric for microservices
    "access_token_expire": 900,  # 15 minutes
    "refresh_token_expire": 604800,  # 7 days
    "issuer": "https://auth.example.com",
    "audience": "https://api.example.com",
}

# Token validation
import jwt
from jwt import PyJWKClient

jwks_client = PyJWKClient("https://auth.example.com/.well-known/jwks.json")

def validate_token(token: str) -> dict:
    signing_key = jwks_client.get_signing_key_from_jwt(token)
    
    return jwt.decode(
        token,
        signing_key.key,
        algorithms=["RS256"],
        issuer=JWT_CONFIG["issuer"],
        audience=JWT_CONFIG["audience"],
    )
```

### API Key Security

```python
import secrets
import hashlib

# Generate secure API key
def generate_api_key() -> tuple[str, str]:
    """Returns (displayable_key, hashed_key)"""
    key = secrets.token_urlsafe(32)
    key_hash = hashlib.sha256(key.encode()).hexdigest()
    return f"sk_live_{key}", key_hash

# Store only the hash
# Display key only once at creation

# Validate API key
def validate_api_key(provided_key: str) -> bool:
    key_hash = hashlib.sha256(provided_key.encode()).hexdigest()
    stored_hash = db.get_api_key_hash(key_hash)  # Lookup by hash
    return secrets.compare_digest(key_hash, stored_hash) if stored_hash else False
```

### Rate Limiting

```python
from fastapi import Request, HTTPException
from fastapi_limiter import FastAPILimiter
from fastapi_limiter.depends import RateLimiter

# Different limits for different endpoints
RATE_LIMITS = {
    "default": "100/minute",
    "auth": "5/minute",
    "search": "30/minute",
    "export": "5/hour",
}

@app.post("/auth/login")
@limiter.limit("5/minute")
async def login(request: Request):
    ...

# Response headers
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640995200
Retry-After: 60  # On 429
```

---

## Infrastructure Security

### Cloud Security Checklist

```markdown
## AWS Security

### IAM
- [ ] Root account MFA enabled
- [ ] No access keys for root
- [ ] Least privilege IAM policies
- [ ] Regular access review
- [ ] Service roles for EC2/Lambda

### Network
- [ ] VPC with private subnets
- [ ] Security groups (deny by default)
- [ ] NACLs for additional layer
- [ ] VPC Flow Logs enabled
- [ ] No public S3 buckets

### Data
- [ ] S3 bucket policies reviewed
- [ ] KMS for encryption
- [ ] RDS encryption enabled
- [ ] Secrets in Secrets Manager
- [ ] CloudTrail enabled

### Compute
- [ ] AMIs hardened
- [ ] Security patches automated
- [ ] No SSH keys in user data
- [ ] Instance metadata v2 required
```

### Container Security

```dockerfile
# Secure Dockerfile
FROM node:20-alpine AS builder
# Build stage...

FROM node:20-alpine
# Don't run as root
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001

# Copy only necessary files
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

# Use non-root user
USER nextjs

# Read-only filesystem where possible
# Set at runtime: docker run --read-only

EXPOSE 3000
CMD ["node", "server.js"]
```

```yaml
# Kubernetes Pod Security
apiVersion: v1
kind: Pod
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1001
    fsGroup: 1001
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
          - ALL
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
```

---

## Security Testing

### Automated Security Scanning

```yaml
# GitHub Actions Security Pipeline
name: Security Scan
on: [push, pull_request]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      # Secret scanning
      - name: TruffleHog
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          
      # Dependency scanning
      - name: Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
          
      # SAST
      - name: CodeQL
        uses: github/codeql-action/analyze@v2
        
      # Container scanning
      - name: Trivy
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'myapp:${{ github.sha }}'
          
      # DAST (on staging)
      - name: OWASP ZAP
        uses: zaproxy/action-full-scan@v0.7.0
        with:
          target: 'https://staging.example.com'
```

### Penetration Testing Checklist

```markdown
## Manual Testing Areas

### Authentication
- [ ] Password brute force
- [ ] Credential stuffing
- [ ] Session fixation
- [ ] Session hijacking
- [ ] JWT attacks (none algorithm, key confusion)

### Authorization
- [ ] Horizontal privilege escalation
- [ ] Vertical privilege escalation
- [ ] IDOR on all endpoints
- [ ] Parameter tampering
- [ ] Force browsing

### Injection
- [ ] SQL injection (all inputs)
- [ ] NoSQL injection
- [ ] Command injection
- [ ] LDAP injection
- [ ] XML injection (XXE)
- [ ] Template injection

### Client-Side
- [ ] Reflected XSS
- [ ] Stored XSS
- [ ] DOM-based XSS
- [ ] CSRF
- [ ] Clickjacking
- [ ] Open redirects

### Business Logic
- [ ] Race conditions
- [ ] Price manipulation
- [ ] Workflow bypass
- [ ] Feature abuse
```

---

## Security Audit Report Template

```markdown
# Security Audit Report

## Executive Summary
- **Application**: [Name]
- **Audit Date**: [Date]
- **Auditor**: [Name/Team]
- **Overall Risk Level**: [Critical/High/Medium/Low]

## Findings Summary
| Severity | Count | Fixed | Open |
|----------|-------|-------|------|
| Critical | 2 | 1 | 1 |
| High | 5 | 3 | 2 |
| Medium | 8 | 5 | 3 |
| Low | 12 | 8 | 4 |

## Critical Findings

### [VULN-001] SQL Injection in User Search
**Severity**: Critical
**CVSS Score**: 9.8
**Status**: Open

**Description**: 
The user search endpoint is vulnerable to SQL injection through the `query` parameter.

**Affected Component**: `/api/users/search`

**Proof of Concept**:
```
GET /api/users/search?query=' OR '1'='1
```

**Impact**: 
Full database compromise, data exfiltration, potential remote code execution.

**Remediation**:
Use parameterized queries:
```python
# Before
cursor.execute(f"SELECT * FROM users WHERE name LIKE '%{query}%'")

# After  
cursor.execute("SELECT * FROM users WHERE name LIKE %s", (f"%{query}%",))
```

**References**:
- CWE-89: SQL Injection
- OWASP SQL Injection Prevention Cheat Sheet

---

## Recommendations

### Immediate (0-7 days)
1. Fix critical SQL injection vulnerability
2. Rotate all database credentials
3. Enable WAF rules for SQL injection

### Short-term (1-4 weeks)
1. Implement parameterized queries throughout
2. Add security headers
3. Enable SAST in CI/CD

### Long-term (1-3 months)
1. Implement comprehensive logging
2. Conduct security training
3. Establish bug bounty program
```

---

## Compliance Quick Reference

### GDPR Checklist

- [ ] Lawful basis for processing documented
- [ ] Privacy notices provided
- [ ] Consent management implemented
- [ ] Data subject rights (access, deletion, portability)
- [ ] Data Processing Agreements with vendors
- [ ] 72-hour breach notification process
- [ ] Data Protection Impact Assessment for high-risk processing
- [ ] Records of processing activities

### SOC 2 Type II Controls

- [ ] Access control policies
- [ ] Background checks
- [ ] Security awareness training
- [ ] Incident response plan
- [ ] Business continuity plan
- [ ] Vendor management
- [ ] Change management
- [ ] Vulnerability management

### PCI-DSS Quick Checks

- [ ] No storage of full track data, CVV, PIN
- [ ] Cardholder data encrypted
- [ ] Network segmentation
- [ ] Strong access control
- [ ] Regular testing
- [ ] Security policies documented

---

## Final Step: Remediation Tracking

After the audit:

1. **Prioritize**: Fix critical/high severity first
2. **Track**: Use issue tracker for all findings
3. **Verify**: Re-test after remediation
4. **Document**: Update security documentation
5. **Learn**: Conduct post-mortem, improve processes

The security audit is complete when all critical and high-severity findings are remediated and verified.
