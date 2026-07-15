---
name: scalability
description: >-
  -- Partial index: index only what you query CREATE INDEX idxactiveusers ON users(email) WHERE status = 'active';  ### 2. Fix N+1 Queries typescript // BAD: 1 + N queries const users = await prisma.user.findMany(); for (const u of users) u.posts = await prisma.post.findMany({ where: { authorId: u.id } });. Use when the user asks about scalability, needs this workflow, or requests related deliverables.
---

# Software Scalability
You are an expert at building systems that handle growth without rewriting. You focus on identifying real bottlenecks before optimizing, and you choose the simplest solution that solves the actual problem.
Read the detailed reference files in \`\$\{CLAUDE_SKILL_DIR\}\` for comprehensive patterns:
- \`database-scaling.md\` — Indexing, query optimization, connection pooling, read replicas, partitioning, sharding, N+1 prevention
- \`caching-and-queues.md\` — Redis patterns, cache invalidation, message queues, async processing, event-driven architecture
- \`api-and-services.md\` — Pagination, rate limiting, circuit breakers, graceful shutdown, load balancing, stateless design
- \`infrastructure.md\` — Kubernetes autoscaling, serverless patterns, CDN caching, deployments, health checks, observability
## The Scalability Mindset
\*\*Rule #1: Don't optimize what you haven't measured.\*\* Profile first, then fix the actual bottleneck.
### Bottleneck Identification Flow
```plain text
Slow response times?
  ↓
Where is time spent?
  ├─ Database (>50% of request time) → See database-scaling.md
  │   ├─ Missing index → Add targeted index
  │   ├─ N+1 queries → Use eager loading / joins
  │   ├─ Full table scans → Add WHERE clauses, pagination
  │   └─ Connection exhaustion → Add connection pooling
  ├─ External API calls → See api-and-services.md
  │   ├─ Slow downstream → Add caching or circuit breaker
  │   └─ Too many calls → Batch or queue
  ├─ CPU-bound computation → See infrastructure.md
  │   ├─ Can parallelize → Worker threads / cluster mode
  │   └─ Can defer → Move to background queue
  └─ Memory pressure → Profile allocations
      ├─ Large payloads → Stream instead of buffer
      └─ Memory leaks → Heap snapshot analysis
```
## Quick Wins — The 80/20 of Scaling
These solve most scaling problems before you need anything complex:
### 1. Add the Right Index
```sql
-- Compound index: equality fields first, then range, then sort
CREATE INDEX idx_orders_lookup
ON orders(customer_id, status, created_at DESC);

-- Partial index: index only what you query
CREATE INDEX idx_active_users
ON users(email) WHERE status = 'active';
```
### 2. Fix N+1 Queries
```typescript
// BAD: 1 + N queries
const users = await prisma.user.findMany();
for (const u of users) u.posts = await prisma.post.findMany({ where: { authorId: u.id } });

// GOOD: 2 queries total
const users = await prisma.user.findMany({ include: { posts: true } });
```
### 3. Add Caching Where It Matters
```typescript
async function getUser(id: string) {
  const cached = await redis.get(`user:${id}`);
  if (cached) return JSON.parse(cached);

  const user = await db.user.findUnique({ where: { id } });
  await redis.setex(`user:${id}`, 300, JSON.stringify(user)); // 5min TTL
  return user;
}
```
### 4. Use Cursor Pagination
```typescript
// Offset pagination degrades: O(offset + limit) — page 1000 scans 100K rows
// Cursor pagination is constant: O(limit) regardless of page

const results = await prisma.post.findMany({
  take: 20,
  cursor: lastId ? { id: lastId } : undefined,
  skip: lastId ? 1 : 0,
  orderBy: { id: 'asc' }
});
```
### 5. Queue Heavy Work
```typescript
// Instead of processing inline (blocks response)
app.post('/upload', async (req, res) => {
  await queue.add('process-upload', { fileId: req.body.fileId });
  res.json({ status: 'queued' }); // Respond immediately
});
```
### 6. Connection Pooling
```plain text
Pool size = (CPU cores × 2) + 1
4 cores → 9 connections
8 cores → 17 connections
```
## Scaling Decision Matrix
\| Symptom \| First Try \| Then Try \| Last Resort \|<br>\|---------\|-----------\|----------\|-------------\|<br>\| Slow queries \| Add indexes, fix N+1 \| Read replicas, caching \| Sharding \|<br>\| High DB connections \| Connection pooling \| PgBouncer/ProxySQL \| Read replicas \|<br>\| API response time \| Caching, pagination \| Async processing \| Microservices \|<br>\| Traffic spikes \| Rate limiting, CDN \| Auto-scaling (HPA) \| Queue-based load leveling \|<br>\| CPU saturation \| Worker threads, optimize code \| Horizontal scaling \| Vertical scaling \|<br>\| Memory pressure \| Stream large data, fix leaks \| Increase instance size \| Offload to external cache \|
## Thresholds — When to Act
\| Metric \| Healthy \| Warning \| Critical \|<br>\|--------\|---------\|---------\|----------\|<br>\| p99 latency \| \< 200ms \| 200-500ms \| \> 500ms \|<br>\| DB cache hit ratio \| \> 99% \| 95-99% \| \< 95% \|<br>\| DB connection utilization \| \< 60% \| 60-80% \| \> 80% \|<br>\| CPU utilization \| \< 50% \| 50-70% \| \> 70% \|<br>\| Memory utilization \| \< 60% \| 60-80% \| \> 80% \|<br>\| Error rate \| \< 0.1% \| 0.1-1% \| \> 1% \|<br>\| Queue depth \| Stable \| Growing \| Growing fast \|<br>\| Read:write ratio \| N/A \| \> 10:1 consider replicas \| \> 50:1 must have replicas \|
## Critical Rules
1. \*\*Measure before optimizing\*\* — Use EXPLAIN ANALYZE, profilers, APM tools; gut feelings are wrong<br>2. \*\*Optimize the bottleneck\*\* — 10x improvement on a non-bottleneck = 0x improvement<br>3. \*\*Start simple, scale when needed\*\* — Single DB → read replicas → sharding (not the reverse)<br>4. \*\*Cache reads, queue writes\*\* — Reads are cheap to cache; writes benefit from async processing<br>5. \*\*Stateless by default\*\* — Session state in Redis/DB, not in memory; enables horizontal scaling<br>6. \*\*Fail gracefully\*\* — Circuit breakers on external calls; timeout everything; have fallbacks<br>7. \*\*Index surgically\*\* — Every index costs write performance; only index what queries actually use<br>8. \*\*Paginate everything\*\* — No unbounded queries; cursor \> offset for large datasets<br>9. \*\*Pool connections\*\* — Opening DB connections is expensive (5-50ms); reuse them<br>10. \*\*Observe everything\*\* — P99 latency, error rate, throughput, saturation; you can't fix what you can't see
Use \`\$ARGUMENTS\` to focus on a specific scaling area. Read the relevant reference file before writing code.
---


## Output format
- Lead with the result the user asked for.
- Use clear headings and bullet lists where helpful.
- Call out assumptions and open questions at the end.
- Stay specific to the Scalability workflow; avoid generic filler.
