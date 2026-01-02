---
name: performance-optimization
description: |
  Analyze and optimize application performance across the full stack. Embodies expertise of a Principal Performance Engineer with 15+ years optimizing systems handling millions of requests per second.
  Use when: user requests performance analysis, optimization, profiling, load testing, or needs to diagnose performance issues.
  Outputs: Performance analysis reports, optimization recommendations, profiling results, load test plans, and implementation guides.
  Keywords: performance, optimization, profiling, benchmarking, latency, throughput, caching, load testing, Core Web Vitals, APM
license: Complete terms in LICENSE.txt
---

# Performance Optimization Skill

**Expertise Level**: Principal Performance Engineer (15+ years equivalent)

This skill embodies the methodology of performance engineering teams at companies like Netflix, Google, and Cloudflare—where milliseconds matter and systems handle millions of concurrent users.

---

## Related Skills

- **[database-design](../database-design)**: Optimize database queries and schema
- **[api-design](../api-design)**: Design performant APIs
- **[devops-automation](../devops-automation)**: Implement performance monitoring
- **[frontend-design](../frontend-design)**: Optimize frontend performance

---

## Core Philosophy

**Measure first, optimize second.** Every performance decision should:

1. **Be Data-Driven**: Profile before optimizing—don't guess
2. **Target Bottlenecks**: Focus on the critical path
3. **Consider Tradeoffs**: Speed vs. cost vs. complexity
4. **Be Sustainable**: Optimizations shouldn't create technical debt
5. **Be Validated**: Benchmark before and after changes

---

## Performance Analysis Process

### Phase 1: Establish Baseline

```markdown
## Baseline Checklist

### Define Metrics
- [ ] P50, P95, P99 latency targets
- [ ] Throughput requirements (RPS)
- [ ] Error rate thresholds
- [ ] Resource utilization bounds
- [ ] Business KPIs affected by performance

### Measure Current State
- [ ] Capture metrics under realistic load
- [ ] Document current performance numbers
- [ ] Identify peak traffic patterns
- [ ] Map critical user journeys
- [ ] Establish monitoring dashboards

### Set Goals
| Metric | Current | Target | Priority |
|--------|---------|--------|----------|
| P50 latency | [X]ms | [Y]ms | High |
| P99 latency | [X]ms | [Y]ms | High |
| Throughput | [X] RPS | [Y] RPS | Medium |
| Error rate | [X]% | [Y]% | Critical |
```

### Phase 2: Identify Bottlenecks

```
┌─────────────────────────────────────────────────────────────────┐
│                    Performance Investigation Flow               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │  Is it slow?    │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
     ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
     │   Frontend  │ │   Network   │ │   Backend   │
     │   Slow?     │ │   Slow?     │ │   Slow?     │
     └──────┬──────┘ └──────┬──────┘ └──────┬──────┘
            │              │              │
            ▼              ▼              ▼
     • Large bundles  • DNS lookup   • CPU bound
     • Render blocking• SSL/TLS      • I/O bound
     • Layout shifts  • Latency      • Memory
     • Heavy JS       • Bandwidth    • Locks/contention
```

---

## Frontend Performance

### Core Web Vitals Optimization

```markdown
## Core Web Vitals Targets

| Metric | Good | Needs Work | Poor |
|--------|------|------------|------|
| **LCP** (Largest Contentful Paint) | ≤2.5s | 2.5-4s | >4s |
| **FID** (First Input Delay) | ≤100ms | 100-300ms | >300ms |
| **CLS** (Cumulative Layout Shift) | ≤0.1 | 0.1-0.25 | >0.25 |
| **INP** (Interaction to Next Paint) | ≤200ms | 200-500ms | >500ms |
| **TTFB** (Time to First Byte) | ≤800ms | 800-1800ms | >1800ms |
```

### JavaScript Optimization

```javascript
// ❌ SLOW: Large bundle, blocking render
import { everything } from 'massive-library';

// ✅ FAST: Tree-shaking friendly import
import { specificFunction } from 'massive-library/specificModule';

// ❌ SLOW: Synchronous, blocking
const data = heavyComputation(largeDataset);

// ✅ FAST: Web Worker for heavy computation
const worker = new Worker('heavy-computation.js');
worker.postMessage(largeDataset);
worker.onmessage = (e) => handleResult(e.data);

// ❌ SLOW: Forced synchronous layout
elements.forEach(el => {
  const height = el.offsetHeight; // Triggers layout
  el.style.height = height + 10 + 'px'; // Triggers layout again
});

// ✅ FAST: Batch reads, then writes
const heights = elements.map(el => el.offsetHeight); // Batch read
elements.forEach((el, i) => {
  el.style.height = heights[i] + 10 + 'px'; // Batch write
});

// ❌ SLOW: Multiple event handlers
document.querySelectorAll('.button').forEach(btn => {
  btn.addEventListener('click', handler);
});

// ✅ FAST: Event delegation
document.addEventListener('click', (e) => {
  if (e.target.matches('.button')) handler(e);
});
```

### Image Optimization

```html
<!-- ❌ SLOW: Large, unoptimized image -->
<img src="hero.png" alt="Hero">

<!-- ✅ FAST: Responsive, lazy-loaded, modern format -->
<img 
  src="hero-800.webp"
  srcset="
    hero-400.webp 400w,
    hero-800.webp 800w,
    hero-1200.webp 1200w
  "
  sizes="(max-width: 600px) 400px, (max-width: 1200px) 800px, 1200px"
  loading="lazy"
  decoding="async"
  alt="Hero"
  width="1200"
  height="600"
>

<!-- For above-the-fold images, use fetchpriority -->
<img 
  src="hero-800.webp"
  fetchpriority="high"
  loading="eager"
  alt="Hero"
>
```

### CSS Optimization

```css
/* ❌ SLOW: Expensive selectors */
div.container > ul li a.link:hover { }
*::before { }
[class^="icon-"] { }

/* ✅ FAST: Simple, specific selectors */
.nav-link:hover { }
.icon-home { }

/* ❌ SLOW: Causes layout/reflow */
.animate {
  width: 100px;
  height: 100px;
  left: 10px;
  top: 10px;
}

/* ✅ FAST: GPU-accelerated properties */
.animate {
  transform: translate(10px, 10px) scale(1.1);
  opacity: 0.8;
}

/* Critical CSS: inline in <head> for above-fold content */
/* Non-critical: load async */
<link rel="preload" href="styles.css" as="style" onload="this.rel='stylesheet'">
```

### Bundle Optimization

```javascript
// webpack.config.js optimization
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
        common: {
          minChunks: 2,
          priority: -10,
          reuseExistingChunk: true,
        },
      },
    },
    runtimeChunk: 'single',
    moduleIds: 'deterministic',
  },
};

// Dynamic imports for code splitting
// ❌ SLOW: Import everything upfront
import { HeavyComponent } from './HeavyComponent';

// ✅ FAST: Lazy load when needed
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

// ✅ Route-based code splitting
const Dashboard = lazy(() => import('./routes/Dashboard'));
const Settings = lazy(() => import('./routes/Settings'));
```

---

## Backend Performance

### Profiling Techniques

```python
# Python profiling with cProfile
import cProfile
import pstats

def profile_function():
    profiler = cProfile.Profile()
    profiler.enable()
    
    # Code to profile
    result = expensive_operation()
    
    profiler.disable()
    stats = pstats.Stats(profiler)
    stats.sort_stats('cumulative')
    stats.print_stats(20)  # Top 20 functions
    
    return result

# Memory profiling
from memory_profiler import profile

@profile
def memory_intensive_function():
    data = [i ** 2 for i in range(1000000)]
    return sum(data)

# Line-by-line profiling
from line_profiler import LineProfiler

lp = LineProfiler()
lp_wrapper = lp(target_function)
lp_wrapper()
lp.print_stats()
```

```javascript
// Node.js profiling
// Start with: node --inspect app.js

// Clinic.js for diagnosis
// npm install -g clinic
// clinic doctor -- node app.js
// clinic flame -- node app.js
// clinic bubbleprof -- node app.js

// Built-in profiler
const { performance, PerformanceObserver } = require('perf_hooks');

const obs = new PerformanceObserver((items) => {
  items.getEntries().forEach((entry) => {
    console.log(`${entry.name}: ${entry.duration}ms`);
  });
});
obs.observe({ entryTypes: ['measure'] });

performance.mark('start');
await expensiveOperation();
performance.mark('end');
performance.measure('expensiveOperation', 'start', 'end');
```

### N+1 Query Prevention

```python
# ❌ SLOW: N+1 queries
def get_orders_with_items():
    orders = Order.objects.all()  # 1 query
    for order in orders:
        items = order.items.all()  # N queries
    return orders

# ✅ FAST: Eager loading
def get_orders_with_items():
    orders = Order.objects.prefetch_related('items').all()  # 2 queries
    return orders

# ✅ FAST: Select related for foreign keys
def get_orders_with_customer():
    orders = Order.objects.select_related('customer').all()  # 1 query with JOIN
    return orders

# SQLAlchemy equivalent
# ❌ SLOW
orders = session.query(Order).all()
for order in orders:
    print(order.customer.name)  # N+1

# ✅ FAST
orders = session.query(Order).options(joinedload(Order.customer)).all()
```

### Caching Strategies

```python
from functools import lru_cache
import redis
import hashlib
import json

# In-memory caching (process-local)
@lru_cache(maxsize=1000)
def expensive_computation(param):
    # Results cached per process
    return compute(param)

# Distributed caching with Redis
redis_client = redis.Redis(host='localhost', port=6379, db=0)

def cache_key(func_name: str, *args, **kwargs) -> str:
    """Generate deterministic cache key."""
    key_data = json.dumps({'args': args, 'kwargs': kwargs}, sort_keys=True)
    return f"{func_name}:{hashlib.md5(key_data.encode()).hexdigest()}"

def cached(ttl_seconds: int = 300):
    """Redis caching decorator."""
    def decorator(func):
        def wrapper(*args, **kwargs):
            key = cache_key(func.__name__, *args, **kwargs)
            
            # Try cache first
            cached_result = redis_client.get(key)
            if cached_result:
                return json.loads(cached_result)
            
            # Compute and cache
            result = func(*args, **kwargs)
            redis_client.setex(key, ttl_seconds, json.dumps(result))
            return result
        return wrapper
    return decorator

@cached(ttl_seconds=3600)
def get_user_recommendations(user_id: str):
    return compute_recommendations(user_id)

# Cache invalidation patterns
def invalidate_user_cache(user_id: str):
    """Invalidate all cache keys for a user."""
    pattern = f"*user:{user_id}*"
    for key in redis_client.scan_iter(pattern):
        redis_client.delete(key)
```

### Connection Pooling

```python
# Database connection pooling
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool

engine = create_engine(
    'postgresql://user:pass@localhost/db',
    poolclass=QueuePool,
    pool_size=10,          # Maintained connections
    max_overflow=20,       # Extra connections when busy
    pool_timeout=30,       # Seconds to wait for connection
    pool_recycle=3600,     # Recycle connections after 1 hour
    pool_pre_ping=True,    # Test connection health
)

# HTTP connection pooling with requests
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

session = requests.Session()
retry_strategy = Retry(
    total=3,
    backoff_factor=0.5,
    status_forcelist=[500, 502, 503, 504]
)
adapter = HTTPAdapter(
    pool_connections=10,
    pool_maxsize=10,
    max_retries=retry_strategy
)
session.mount("http://", adapter)
session.mount("https://", adapter)
```

### Async/Concurrency

```python
import asyncio
import aiohttp

# ❌ SLOW: Sequential requests
def fetch_all_sequential(urls):
    results = []
    for url in urls:
        response = requests.get(url)
        results.append(response.json())
    return results

# ✅ FAST: Concurrent requests
async def fetch_all_concurrent(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_one(session, url) for url in urls]
        return await asyncio.gather(*tasks)

async def fetch_one(session, url):
    async with session.get(url) as response:
        return await response.json()

# With rate limiting
from asyncio import Semaphore

async def fetch_with_limit(urls, max_concurrent=10):
    semaphore = Semaphore(max_concurrent)
    
    async def fetch_limited(session, url):
        async with semaphore:
            return await fetch_one(session, url)
    
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_limited(session, url) for url in urls]
        return await asyncio.gather(*tasks)
```

---

## Database Performance

### Query Optimization

```sql
-- Analyze query execution
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT u.name, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at > '2024-01-01'
GROUP BY u.id
ORDER BY order_count DESC
LIMIT 10;

-- ❌ SLOW: Full table scan
SELECT * FROM orders WHERE YEAR(created_at) = 2024;

-- ✅ FAST: Uses index
SELECT * FROM orders 
WHERE created_at >= '2024-01-01' 
  AND created_at < '2025-01-01';

-- ❌ SLOW: OR with different columns
SELECT * FROM users WHERE email = 'x' OR phone = 'y';

-- ✅ FAST: UNION for separate index usage
SELECT * FROM users WHERE email = 'x'
UNION
SELECT * FROM users WHERE phone = 'y';

-- ❌ SLOW: SELECT * with large rows
SELECT * FROM products WHERE category_id = 5;

-- ✅ FAST: Select only needed columns
SELECT id, name, price FROM products WHERE category_id = 5;

-- ❌ SLOW: Counting with full scan
SELECT COUNT(*) FROM large_table;

-- ✅ FAST: Approximate count
SELECT reltuples::bigint AS estimate
FROM pg_class WHERE relname = 'large_table';
```

### Index Strategies

```sql
-- Covering index (includes all needed columns)
CREATE INDEX idx_orders_user_covering 
ON orders (user_id, status)
INCLUDE (total, created_at);

-- Partial index (only index subset)
CREATE INDEX idx_orders_pending 
ON orders (created_at)
WHERE status = 'pending';

-- Expression index
CREATE INDEX idx_users_email_lower 
ON users (LOWER(email));

-- Composite index (order matters!)
-- Supports: (user_id), (user_id, status), (user_id, status, created_at)
-- Does NOT support: (status), (status, created_at)
CREATE INDEX idx_orders_composite 
ON orders (user_id, status, created_at DESC);

-- Monitor index usage
SELECT 
    indexrelname,
    idx_scan,
    idx_tup_read,
    idx_tup_fetch
FROM pg_stat_user_indexes
WHERE schemaname = 'public'
ORDER BY idx_scan DESC;

-- Find unused indexes
SELECT 
    indexrelname,
    pg_size_pretty(pg_relation_size(indexrelid))
FROM pg_stat_user_indexes
WHERE idx_scan = 0
  AND indexrelname NOT LIKE '%_pkey';
```

---

## Load Testing

### Load Test Plan

```markdown
## Load Test Plan Template

### Test Objectives
- Determine maximum throughput
- Identify breaking points
- Validate SLA under load
- Find performance bottlenecks

### Load Profiles

| Phase | Duration | Users | Ramp-up |
|-------|----------|-------|---------|
| Warm-up | 5 min | 10 | Immediate |
| Ramp | 10 min | 10→100 | Linear |
| Steady | 30 min | 100 | Steady |
| Spike | 5 min | 500 | Immediate |
| Recovery | 10 min | 100 | Immediate |

### Key Scenarios
1. User login flow (20% of traffic)
2. Product search (40% of traffic)
3. Add to cart (25% of traffic)
4. Checkout (15% of traffic)

### Success Criteria
| Metric | Threshold |
|--------|-----------|
| P99 latency | <500ms |
| Error rate | <0.1% |
| Throughput | >1000 RPS |
```

### k6 Load Test Script

```javascript
// load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate, Trend } from 'k6/metrics';

// Custom metrics
const errorRate = new Rate('errors');
const loginDuration = new Trend('login_duration');

export const options = {
  stages: [
    { duration: '2m', target: 50 },   // Ramp up
    { duration: '5m', target: 50 },   // Steady state
    { duration: '2m', target: 100 },  // Spike
    { duration: '5m', target: 100 },  // Steady spike
    { duration: '2m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500', 'p(99)<1000'],
    errors: ['rate<0.01'],
  },
};

const BASE_URL = __ENV.BASE_URL || 'http://localhost:3000';

export default function () {
  // Login flow
  const loginStart = Date.now();
  const loginRes = http.post(`${BASE_URL}/api/login`, {
    email: 'test@example.com',
    password: 'password123',
  });
  loginDuration.add(Date.now() - loginStart);
  
  check(loginRes, {
    'login successful': (r) => r.status === 200,
    'has token': (r) => r.json('token') !== undefined,
  }) || errorRate.add(1);

  const token = loginRes.json('token');
  const headers = { Authorization: `Bearer ${token}` };

  sleep(1);

  // Product search
  const searchRes = http.get(
    `${BASE_URL}/api/products?search=widget`,
    { headers }
  );
  
  check(searchRes, {
    'search successful': (r) => r.status === 200,
    'has results': (r) => r.json('data').length > 0,
  }) || errorRate.add(1);

  sleep(2);

  // Add to cart
  const addCartRes = http.post(
    `${BASE_URL}/api/cart`,
    JSON.stringify({ productId: 'prod_123', quantity: 1 }),
    { headers, headers: { 'Content-Type': 'application/json' } }
  );
  
  check(addCartRes, {
    'add to cart successful': (r) => r.status === 201,
  }) || errorRate.add(1);

  sleep(1);
}

export function handleSummary(data) {
  return {
    'summary.json': JSON.stringify(data),
    stdout: textSummary(data, { indent: ' ', enableColors: true }),
  };
}
```

### Locust Load Test

```python
# locustfile.py
from locust import HttpUser, task, between, events
import logging

class WebsiteUser(HttpUser):
    wait_time = between(1, 3)
    
    def on_start(self):
        """Login on start."""
        response = self.client.post("/api/login", json={
            "email": "test@example.com",
            "password": "password123"
        })
        self.token = response.json().get("token")
        self.client.headers = {"Authorization": f"Bearer {self.token}"}
    
    @task(4)
    def search_products(self):
        """40% of traffic."""
        self.client.get("/api/products?search=widget")
    
    @task(2)
    def view_product(self):
        """20% of traffic."""
        self.client.get("/api/products/prod_123")
    
    @task(2)
    def add_to_cart(self):
        """20% of traffic."""
        self.client.post("/api/cart", json={
            "productId": "prod_123",
            "quantity": 1
        })
    
    @task(1)
    def checkout(self):
        """10% of traffic."""
        self.client.post("/api/checkout")
    
    @task(1)
    def view_profile(self):
        """10% of traffic."""
        self.client.get("/api/profile")

@events.test_stop.add_listener
def on_test_stop(environment, **kwargs):
    """Log summary on test completion."""
    if environment.stats.total.fail_ratio > 0.01:
        logging.error(f"Test failed: Error rate {environment.stats.total.fail_ratio:.2%}")
```

---

## Monitoring & Alerting

### Key Metrics Dashboard

```yaml
# Grafana dashboard panels

## RED Method (Request-oriented)
- Rate: Requests per second
- Errors: Error rate percentage
- Duration: Latency percentiles (p50, p95, p99)

## USE Method (Resource-oriented)
- Utilization: CPU, memory, disk, network usage %
- Saturation: Queue depth, thread pool usage
- Errors: Hardware/software errors

## Custom Metrics
panels:
  - title: "Request Rate"
    query: "rate(http_requests_total[5m])"
    
  - title: "Error Rate"
    query: "rate(http_requests_total{status=~'5..'}[5m]) / rate(http_requests_total[5m])"
    alert:
      threshold: 0.01
      
  - title: "P99 Latency"
    query: "histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))"
    alert:
      threshold: 1.0
      
  - title: "Database Connection Pool"
    query: "db_connections_active / db_connections_max"
    alert:
      threshold: 0.9
```

### APM Integration

```python
# OpenTelemetry instrumentation
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.sqlalchemy import SQLAlchemyInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor

# Initialize tracing
trace.set_tracer_provider(TracerProvider())
tracer = trace.get_tracer(__name__)

# Export to collector
otlp_exporter = OTLPSpanExporter(endpoint="http://collector:4317")
trace.get_tracer_provider().add_span_processor(
    BatchSpanProcessor(otlp_exporter)
)

# Auto-instrument frameworks
FlaskInstrumentor().instrument_app(app)
SQLAlchemyInstrumentor().instrument(engine=engine)
RequestsInstrumentor().instrument()

# Manual instrumentation for custom spans
@tracer.start_as_current_span("process_order")
def process_order(order_id: str):
    span = trace.get_current_span()
    span.set_attribute("order.id", order_id)
    
    with tracer.start_as_current_span("validate_order"):
        validate(order_id)
    
    with tracer.start_as_current_span("charge_payment"):
        charge(order_id)
    
    with tracer.start_as_current_span("send_confirmation"):
        notify(order_id)
```

---

## Performance Optimization Checklist

### Frontend

- [ ] Bundle size analyzed and optimized
- [ ] Code splitting implemented
- [ ] Images optimized (format, size, lazy loading)
- [ ] Critical CSS inlined
- [ ] Third-party scripts audited
- [ ] Service worker caching
- [ ] Core Web Vitals passing
- [ ] Lighthouse score >90

### Backend

- [ ] N+1 queries eliminated
- [ ] Database queries optimized with EXPLAIN
- [ ] Appropriate indexes in place
- [ ] Caching strategy implemented
- [ ] Connection pooling configured
- [ ] Async operations for I/O
- [ ] Rate limiting in place
- [ ] Compression enabled

### Infrastructure

- [ ] CDN configured
- [ ] Load balancing in place
- [ ] Auto-scaling configured
- [ ] Monitoring and alerting active
- [ ] Resource limits set
- [ ] Horizontal vs. vertical scaling evaluated
- [ ] Geographic distribution considered

---

## Final Step: Validate Improvements

After optimization:

1. **Benchmark**: Run identical tests before and after
2. **Compare**: Document improvements with exact numbers
3. **Monitor**: Watch metrics in production
4. **Iterate**: Performance is never "done"

The optimization is successful when measured performance meets targets AND no new issues were introduced.
