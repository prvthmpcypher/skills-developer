---
name: graphql-api-designer
description: >-
  Designs GraphQL schemas with type definitions, query/mutation structures, resolver patterns, pagination strategies, and N+1 query prevention. Use when designing GraphQL APIs, optimizing query performance, or migrating from REST to GraphQL.
---

# GraphQL API Designer

A GraphQL schema is a contract that's hard to change once clients depend on it — deprecating a field is possible, but removing one outright breaks every consumer. Design for the shape of the data callers actually need, not a 1:1 mirror of your database tables.

## Workflow

1. **Model around use cases, not tables.** A GraphQL type doesn't need to match a database row — it should represent what a client actually wants to query together. Ask what the frontend/consumer needs to render before designing types.
2. **Use precise scalar and nullability rules.** Mark a field non-null (`!`) only when it's truly always present — over-using non-null is the single most common schema mistake, since it forces breaking changes later when a field turns out to sometimes be absent.
3. **Design pagination explicitly.** Default to cursor-based (Relay-style `edges`/`node`/`pageInfo`) for any list that can grow unbounded — offset-based pagination is simpler but breaks under concurrent inserts/deletes and doesn't scale to large datasets.
4. **Separate queries from mutations clearly**, and design mutation inputs as dedicated input types (`CreateUserInput`) rather than reusing query types — this keeps the API stable as create/update needs diverge from read needs.
5. **Flag N+1 risk explicitly.** Any resolver that fetches a related object per-item in a list (e.g. `author` on every `Post`) needs batching (DataLoader pattern) — call this out in the design even if you're not writing the resolver implementation yet, since it's the most common GraphQL performance bug.
6. **Version through deprecation, not breaking changes.** Use `@deprecated(reason: "...")` on fields being phased out rather than removing them outright while clients may still depend on them.

## What NOT to do

- Don't design a schema that exposes internal database structure directly — abstract it to the shape callers need.
- Don't skip authorization design — GraphQL's flexible querying makes field-level authorization more important than in REST, where per-endpoint gating is more natural. Flag if the user's schema needs field-level access rules and hasn't specified them.

## Output format

Provide the SDL (schema definition language) directly:
```graphql
type Post {
  id: ID!
  title: String!
  author: User!
  comments(first: Int, after: String): CommentConnection!
}
```
Follow with a short note on any N+1 risks, pagination choices, and authorization gaps that need resolver-level handling.

See `references/pagination-patterns.md` for the full Relay cursor-pagination spec.

## Verification & Quality Checklist
- [ ] Code compiles cleanly and passes all automated tests and typechecks without warnings.
- [ ] Edge cases, boundary conditions, and error states handled explicitly.
- [ ] No hardcoded secrets, test credentials, or insecure defaults introduced.
- [ ] Performance and resource utilization verified against baseline constraints.
