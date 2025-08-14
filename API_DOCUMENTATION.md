### Project API and Data Documentation

This repository currently contains data sources in JSON form and does not define code-level exported functions or UI components. This document treats the JSON datasets as the public surface area and provides:

- Data models (schemas and types)
- Relationships
- Example usage (reading from files)
- Optional instructions to serve these datasets as a REST API using json-server, with example requests


## Datasets

- `auth.json`: `{ users: AuthUser[] }`
- `password.json`: `{ users: PasswordUser[] }`
- `post.json`: `Post[]`
- `comments.json`: `Comment[]`
- `product_data.json`: `Product[]`


## Data Models

```ts
// Core Types
export type Identifier = number;

export interface AuthUser {
  id: Identifier;
  name: string;
  email: string;
  password: string;
}

export interface PasswordUser {
  id: Identifier;
  name: string;
  email: string;
  password: string;
}

export interface Post {
  userId: Identifier; // references AuthUser.id (or PasswordUser.id if you choose that source)
  id: Identifier;
  title: string;
  body: string;
}

export interface Comment {
  postId: Identifier; // references Post.id
  id: Identifier;
  name: string;
  email: string;
  body: string;
}

export interface Product {
  id: Identifier;
  name: string;
  category: string;
  description: string;
  price: number; // USD
  image: string; // URL
  inStock: boolean;
}
```

Minimal JSON Schemas (draft-07 style) for validation:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "#AuthUser",
  "type": "object",
  "required": ["id", "name", "email", "password"],
  "properties": {
    "id": { "type": "integer", "minimum": 1 },
    "name": { "type": "string" },
    "email": { "type": "string", "format": "email" },
    "password": { "type": "string", "minLength": 1 }
  }
}
```

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "#Post",
  "type": "object",
  "required": ["userId", "id", "title", "body"],
  "properties": {
    "userId": { "type": "integer", "minimum": 1 },
    "id": { "type": "integer", "minimum": 1 },
    "title": { "type": "string" },
    "body": { "type": "string" }
  }
}
```

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "#Comment",
  "type": "object",
  "required": ["postId", "id", "name", "email", "body"],
  "properties": {
    "postId": { "type": "integer", "minimum": 1 },
    "id": { "type": "integer", "minimum": 1 },
    "name": { "type": "string" },
    "email": { "type": "string", "format": "email" },
    "body": { "type": "string" }
  }
}
```

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "#Product",
  "type": "object",
  "required": ["id", "name", "category", "description", "price", "image", "inStock"],
  "properties": {
    "id": { "type": "integer", "minimum": 1 },
    "name": { "type": "string" },
    "category": { "type": "string" },
    "description": { "type": "string" },
    "price": { "type": "number", "minimum": 0 },
    "image": { "type": "string", "format": "uri" },
    "inStock": { "type": "boolean" }
  }
}
```


## Relationships

- `Post.userId -> AuthUser.id`
- `Comment.postId -> Post.id`


## Example Data Records

```json
// From auth.json
{
  "id": 1,
  "name": "Alice Johnson",
  "email": "alice.johnson@example.com",
  "password": "password123"
}
```

```json
// From post.json
{
  "userId": 1,
  "id": 1,
  "title": "My First Post",
  "body": "This is the body content of the first post."
}
```

```json
// From comments.json
{
  "postId": 1,
  "id": 1,
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "body": "Great post! Thanks for sharing."
}
```

```json
// From product_data.json
{
  "id": 1,
  "name": "Monstera Deliciosa",
  "category": "Indoor Plants",
  "description": "Beautiful split‑leaf houseplant perfect for bright, indirect light.",
  "price": 45.99,
  "image": "https://pixabay.com/photos/monstera-deliciosa-flora-nature-8426717/",
  "inStock": true
}
```


## Usage Examples (Reading From Files)

JavaScript (Node.js):

```javascript
import { readFile } from 'node:fs/promises';

async function loadProducts() {
  const raw = await readFile('./product_data.json', 'utf8');
  /** @type {any[]} */
  const products = JSON.parse(raw);
  const inStock = products.filter(p => p.inStock);
  return inStock;
}

async function loadPostsWithComments() {
  const [postsRaw, commentsRaw] = await Promise.all([
    readFile('./post.json', 'utf8'),
    readFile('./comments.json', 'utf8')
  ]);
  /** @type {any[]} */
  const posts = JSON.parse(postsRaw);
  /** @type {any[]} */
  const comments = JSON.parse(commentsRaw);

  const postIdToComments = comments.reduce((map, c) => {
    (map[c.postId] ||= []).push(c);
    return map;
  }, /** @type {Record<number, any[]>} */ ({}));

  return posts.map(p => ({ ...p, comments: postIdToComments[p.id] || [] }));
}
```

Python:

```python
import json
from pathlib import Path

def load_users():
    data = json.loads(Path('auth.json').read_text())
    return data['users']

def posts_with_comments():
    posts = json.loads(Path('post.json').read_text())
    comments = json.loads(Path('comments.json').read_text())
    by_post = {}
    for c in comments:
        by_post.setdefault(c['postId'], []).append(c)
    return [ { **p, 'comments': by_post.get(p['id'], []) } for p in posts ]
```


## Optional: Serve as a REST API (json-server)

If you want REST endpoints without writing backend code, you can combine these datasets into a single `db.json` and run `json-server`.

1) Create `db.json`:

```json
{
  "users": { "$import": "./auth.json#users" },
  "passwordUsers": { "$import": "./password.json#users" },
  "posts": { "$import": "./post.json#" },
  "comments": { "$import": "./comments.json#" },
  "products": { "$import": "./product_data.json#" }
}
```

Note: `json-server` does not support `$import` natively. Instead, actually paste the arrays under each key, or write a small script to merge files into `db.json`.

2) Run:

```bash
npx --yes json-server --watch db.json --port 3000
```

3) Example endpoints:

- GET `http://localhost:3000/users`
- GET `http://localhost:3000/posts?userId=1`
- GET `http://localhost:3000/comments?postId=1`
- GET `http://localhost:3000/products?inStock=true`

Example request/response:

```bash
curl -s 'http://localhost:3000/products?inStock=true' | jq '.[0]'
```

```json
{
  "id": 1,
  "name": "Monstera Deliciosa",
  "category": "Indoor Plants",
  "description": "Beautiful split‑leaf houseplant perfect for bright, indirect light.",
  "price": 45.99,
  "image": "https://pixabay.com/photos/monstera-deliciosa-flora-nature-8426717/",
  "inStock": true
}
```


## Notes and Conventions

- IDs are integers and unique per collection.
- `price` is a number (assumed USD). No currency field is present.
- Password fields are present in `auth.json` and `password.json` for testing purposes only. Do not commit real credentials.
- Emails follow typical formats but are not guaranteed unique across datasets.


## Changelog

- v1.0: Initial documentation of data models, relationships, examples, and optional REST serving instructions.
