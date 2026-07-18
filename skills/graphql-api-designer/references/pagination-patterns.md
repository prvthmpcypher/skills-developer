# Relay-style cursor pagination

```graphql
type PostConnection {
  edges: [PostEdge!]!
  pageInfo: PageInfo!
}

type PostEdge {
  node: Post!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}

type Query {
  posts(first: Int, after: String, last: Int, before: String): PostConnection!
}
```

Cursors are typically an opaque base64-encoded string (often the item's ID or a composite sort key), not a raw offset — this is what makes cursor pagination stable under concurrent inserts/deletes, unlike `LIMIT/OFFSET`.

For a DataLoader-style batching fix to N+1 queries: batch all resolver calls for a given field within a single tick into one underlying query keyed by ID, rather than issuing one query per parent object.
