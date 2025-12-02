# Phase 5 — Database Mastery (Weeks 19–22)

## Objective
Master relational and NoSQL databases used in modern full stack apps: SQL Server/Postgres for relational needs and MongoDB for document storage. Learn design, indexing, partitioning, and performance tuning.

## Outcomes
- Design normalized schemas, create efficient indexes.
- Use EF Core with multiple providers.
- Model and query documents in MongoDB and understand trade-offs.

## Week-by-week Plan
Week 19 — SQL Server Deep Dive
- Indexing strategies, execution plans, stored procedures.
- Partitioning and maintenance (backups, restores).

Week 20 — PostgreSQL & Advanced Features
- JSONB usage, full-text search, extensions (pg_trgm).
- Differences vs SQL Server and when to prefer Postgres.

Week 21 — MongoDB Introduction
- Data modeling for documents, aggregation pipelines.
- Index types and shard basics.

Week 22 — Multi-DB Patterns & Caching
- CQRS basics, when to use polyglot persistence.
- Caching strategies (Redis overview).

## Projects / Exercises
- Multi-database App: relational for transactional data, Mongo for denormalized analytics.
- Build complex aggregation queries and optimize with indexes.

## Acceptance Checklist
- [ ] At least one project uses Postgres or SQL Server with performance tuning.
- [ ] Implemented a MongoDB-backed feature with aggregation pipeline.
- [ ] Demonstrated caching improves response times (Redis mock).

## Resources (Focused)
- SQL basics & tutorials: https://sqlbolt.com/ and https://www.sqltutorial.org/
- SQL Server indexing & execution plans: https://learn.microsoft.com/sql/relational-databases/performance/execution-plans
- PostgreSQL docs: https://www.postgresql.org/docs/
- MongoDB University: https://university.mongodb.com/
- Redis docs: https://redis.io/docs/getting-started/

---

## Interview Topics & Most-Asked Questions (Phase 5)

1. Indexes and Query Optimization
   - Q: What are indexes, how do they work, and when can they hurt performance?
   - Learn: SQL Server: https://learn.microsoft.com/sql/relational-databases/indexes/indexes-overview  
     Postgres: https://www.postgresql.org/docs/current/indexes.html

2. Transaction Isolation Levels
   - Q: Explain isolation levels and phenomena (dirty reads, non-repeatable reads, phantom reads).
   - Learn: https://www.postgresql.org/docs/current/transaction-iso.html

3. Read & Interpret EXPLAIN / Execution Plan
   - Q: Show how you'd optimize a slow query using EXPLAIN and indexes.
   - Learn: https://www.postgresql.org/docs/current/using-explain.html

4. MongoDB Data Modeling: embed vs reference
   - Q: When to embed documents vs. reference and how to design for queries?
   - Learn: https://docs.mongodb.com/manual/core/data-modeling-introduction/

5. Caching Patterns & Redis Pitfalls
   - Q: How to avoid cache stampede, what eviction policies to use?
   - Learn: https://redis.io/docs/manual/patterns/ and https://redis.io/docs/manual/eviction/

Practice approach:
- Take a slow-query example, add indexes or rewrite joins, and prepare before/after metrics to show in interviews.