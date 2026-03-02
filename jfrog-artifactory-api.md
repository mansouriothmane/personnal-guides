# JFrog Artifactory REST API

Minimal reference for the **Storage API** and the **Artifactory Query Language (AQL) API**.

> Full official docs: https://jfrog.com/help/r/jfrog-rest-apis

---

## Authentication

All requests require an `Authorization` header.

| Scheme | Header |
|--------|--------|
| Access token | `Authorization: Bearer <token>` |
| Username + password / API key | `Authorization: Basic base64(<user>:<password>)` |

The base URL for every endpoint below is `https://<host>/artifactory`.

---

## Storage API

> https://jfrog.com/help/r/jfrog-rest-apis/folder-info

### Folder Info

List the direct children of a repository root or a folder inside it.

```HTTP
GET /api/storage/{repo}[/{path}]
Accept: application/json
```

**Response**

```json
{
  "repo": "my-repo",
  "path": "/my-folder",
  "uri": "https://host/artifactory/api/storage/my-repo/my-folder",
  "children": [
    { "uri": "/sub-folder", "folder": true },
    { "uri": "/artifact-1.0.0.tgz", "folder": false }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `repo` | `string` | Repository key |
| `path` | `string` | Path of this folder within the repo |
| `children[].uri` | `string` | Relative URI of the child (always starts with `/`) |
| `children[].folder` | `boolean` | `true` for sub-folders, `false` for files |

### Direct Download

Download an artifact by its logical path (no `/api/storage/` prefix):

```HTTP
GET /{repo}/{path/to/file}
```

---

## AQL API

> https://jfrog.com/help/r/jfrog-rest-apis/artifactory-query-language

AQL lets you search artifacts and their metadata (properties, stats, builds, etc.).

### Endpoint

```HTTP
POST /api/search/aql
Content-Type: text/plain
```

The request body is a raw AQL string — **not** JSON.

### Syntax

```typescript
items.find(<criteria>)
     [.include(<fields>)]
     [.sort(<sort>)]
     [.limit(<n>)]
     [.offset(<n>)]
```

| Clause | Required | Description |
|--------|----------|-------------|
| `items.find({...})` | Yes | Filter criteria as a JSON object |
| `.include("field", ...)` | No | Fields to return; prefix item properties with `@` |
| `.sort({"$asc"\|"$desc": ["field"]})` | No | Sort order |
| `.limit(n)` | No | Maximum number of results |
| `.offset(n)` | No | Skip first *n* results |

#### Filter operators

| Operator | Example |
|----------|---------|
| Exact match | `{"repo": "my-repo"}` |
| Glob / wildcard | `{"name": {"$match": "*.tgz"}}` |
| AND | `{"$and": [{...}, {...}]}` |
| OR | `{"$or": [{...}, {...}]}` |
| Comparison | `{"size": {"$gt": "1024"}}` |

#### Item properties

Use `@key` in `.include()` to return properties, and `"@key": "value"` in `.find()` to filter by them:

```typescript
items.find({
  "repo": "my-repo",
  "path": "my/path",
  "name": { "$match": "*.tgz" },
  "@my.property": "expected-value"
}).include("repo", "name", "path", "@my.property")
```

### Response

```json
{
  "results": [
    {
      "repo": "my-repo",
      "path": "my/path",
      "name": "artifact-1.0.0.tgz",
      "properties": [
        { "key": "my.property", "value": "expected-value" }
      ]
    }
  ],
  "range": { "start_pos": 0, "end_pos": 1, "total": 1 }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `results[].repo` | `string` | Repository key |
| `results[].path` | `string` | Folder path within the repo |
| `results[].name` | `string` | Artifact filename |
| `results[].properties` | `array` | Item properties (only present when requested via `@key` in `.include()`) |
