# Dgraph GraphQL API

Dgraph provides a native GraphQL API layer that sits directly on top of its distributed graph backend. Unlike adapter-based GraphQL implementations, Dgraph generates a complete GraphQL API — including queries, mutations, and subscriptions — automatically from a type schema you define. This makes Dgraph a truly GraphQL-native database.

## Overview

- **Endpoint:** `https://<your-backend>.us-west-2.aws.cloud.dgraph.io/graphql`
- **Protocol:** HTTP POST (queries and mutations), WebSocket (subscriptions)
- **Schema-Driven:** Define types and Dgraph auto-generates CRUD operations
- **Real-Time:** Native subscription support via WebSockets

## Authentication

Dgraph Cloud supports JWT-based authentication. Requests to the GraphQL endpoint include an `X-Auth-Token` header or a signed JWT depending on the backend configuration.

```
X-Auth-Token: <your-api-key>
```

For anonymous access (if enabled), no token is required.

## Schema Definition

A Dgraph schema is written in GraphQL SDL (Schema Definition Language). Dgraph-specific directives control indexing, authorization, and field behavior.

```graphql
type Product @auth(
  query: { rule: "{$role: {eq: \"USER\"}}" }
) {
  id: ID!
  name: String! @search(by: [term])
  price: Float
  category: String @search(by: [hash])
  createdAt: DateTime
}
```

After uploading the schema to `/admin/schema`, Dgraph generates:

- `getProduct(id: ID!): Product`
- `queryProduct(filter: ..., order: ..., first: Int, offset: Int): [Product]`
- `aggregateProduct(filter: ...): ProductAggregateResult`
- `addProduct(input: [AddProductInput!]!): AddProductPayload`
- `updateProduct(input: UpdateProductInput!): UpdateProductPayload`
- `deleteProduct(filter: ProductFilter!): DeleteProductPayload`

## Key Directives

| Directive | Purpose |
|---|---|
| `@search` | Enables text/term/hash/geo/range search indexes |
| `@auth` | Row-level authorization rules using JWT claims |
| `@id` | Marks a field as a unique identifier |
| `@dgraph` | Maps GraphQL types/fields to Dgraph predicates |
| `@lambdaOnMutate` | Triggers a serverless lambda on mutation events |
| `@cascade` | Filters query results where all requested fields exist |
| `@withSubscription` | Enables real-time subscriptions on a type |

## Queries

Dgraph auto-generates flexible query operations with built-in filtering, ordering, and pagination.

```graphql
query {
  queryProduct(
    filter: { name: { anyofterms: "widget" }, price: { ge: 10.0 } }
    order: { asc: price }
    first: 20
    offset: 0
  ) {
    id
    name
    price
    category
  }
}
```

## Mutations

```graphql
mutation {
  addProduct(input: [
    { name: "Widget Pro", price: 29.99, category: "hardware" }
  ]) {
    product {
      id
      name
    }
  }
}
```

## Subscriptions

Subscriptions require the `@withSubscription` directive on a type and a WebSocket connection.

```graphql
subscription {
  queryProduct(filter: { category: { eq: "hardware" } }) {
    id
    name
    price
  }
}
```

Connect via `wss://<your-backend>.us-west-2.aws.cloud.dgraph.io/graphql` using the `graphql-ws` protocol.

## Admin API

Schema management and cluster administration are handled via the `/admin` endpoint, which also exposes a GraphQL API:

- **Schema upload:** `POST /admin/schema` with the SDL body
- **Schema query:** `GET /admin/schema`
- **Health check:** `GET /health`
- **State:** `GET /state`

## DQL (Dgraph Query Language)

For advanced use cases requiring direct access to the graph backend, Dgraph also exposes DQL (formerly GraphQL±) at the `/query` endpoint. DQL allows traversal-style queries, complex aggregations, and operations not yet expressible in the GraphQL layer.

```
{
  products(func: has(Product.name)) {
    Product.name
    Product.price
    Product.category
  }
}
```

## Error Handling

The GraphQL API returns standard GraphQL error envelopes:

```json
{
  "errors": [
    {
      "message": "couldn't rewrite query: field 'unknownField' not found in type 'Product'",
      "locations": [{ "line": 3, "column": 5 }],
      "extensions": { "code": "Error" }
    }
  ]
}
```

## SDK and Client Support

Any GraphQL client library works with Dgraph Cloud:

- **JavaScript/TypeScript:** `graphql-request`, Apollo Client, urql
- **Python:** `gql`, `python-graphql-client`
- **Go:** `dgo` (official Dgraph Go client)
- **Java:** `dgraph4j` (official Dgraph Java client)
- **cURL:** Standard HTTP POST with `Content-Type: application/json`

## Resources

- [Dgraph Documentation](https://docs.dgraph.io/)
- [GraphQL API Overview](https://docs.dgraph.io/graphql/overview/)
- [Schema Directives Reference](https://docs.dgraph.io/graphql/schema/directives/)
- [Auth Rules](https://docs.dgraph.io/graphql/authorization/authorization-overview/)
- [Dgraph Cloud Console](https://cloud.dgraph.io/)
- [GitHub: dgraph-io/dgraph](https://github.com/dgraph-io/dgraph)
