# API Reference

## Quick Start

Base URL: `http://localhost:3000/api`

## Authentication

All endpoints require authentication unless specified as public.

**Headers:**
```
Authorization: Bearer <jwt_token>
Content-Type: application/json
```

## Endpoints

### Users

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/users` | Get all users | Yes |
| GET | `/users/{id}` | Get user by ID | Yes |
| POST | `/users` | Create user | No |
| PUT | `/users/{id}` | Update user | Yes |
| DELETE | `/users/{id}` | Delete user | Yes |

### Products

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/products` | Get all products | No |
| GET | `/products/{id}` | Get product by ID | No |
| POST | `/products` | Create product | Yes |
| PUT | `/products/{id}` | Update product | Yes |
| DELETE | `/products/{id}` | Delete product | Yes |

### Posts

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/posts` | Get all posts | No |
| GET | `/posts/{id}` | Get post by ID | No |
| POST | `/posts` | Create post | Yes |
| GET | `/posts/{id}/comments` | Get post comments | No |
| POST | `/posts/{id}/comments` | Add comment | No |

## Data Schemas

### User
```json
{
  "id": "integer",
  "name": "string",
  "email": "string",
  "password": "string"
}
```

### Product
```json
{
  "id": "integer",
  "name": "string",
  "category": "string",
  "description": "string",
  "price": "decimal",
  "image": "string",
  "inStock": "boolean"
}
```

### Post
```json
{
  "userId": "integer",
  "id": "integer",
  "title": "string",
  "body": "string"
}
```

### Comment
```json
{
  "postId": "integer",
  "id": "integer",
  "name": "string",
  "email": "string",
  "body": "string"
}
```

## Query Parameters

### Products
- `category`: Filter by category (Indoor Plants, Succulents, Flowering Plants)
- `inStock`: Filter by availability (true/false)
- `minPrice`: Minimum price filter
- `maxPrice`: Maximum price filter

### Posts
- `userId`: Filter by user ID
- `page`: Page number for pagination
- `limit`: Number of items per page

## Response Formats

### Success Response
```json
{
  "data": {...},
  "message": "Success message"
}
```

### Error Response
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Error description"
  }
}
```

### Paginated Response
```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "pages": 10
  }
}
```

## Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `409` - Conflict
- `500` - Internal Server Error

## Examples

### Create User
```bash
curl -X POST "http://localhost:3000/api/users" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
  }'
```

### Get Products by Category
```bash
curl -X GET "http://localhost:3000/api/products?category=Indoor%20Plants"
```

### Add Comment
```bash
curl -X POST "http://localhost:3000/api/posts/1/comments" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Commenter",
    "email": "commenter@example.com",
    "body": "Great post!"
  }'
```

## Rate Limits

- **Public endpoints**: 100 requests per minute
- **Authenticated endpoints**: 1000 requests per minute
- **Admin endpoints**: 5000 requests per minute

## Versioning

Current API version: `v1`

To use a specific version, include it in the URL:
```
http://localhost:3000/api/v1/users
```

---

*For detailed documentation, see [API_DOCUMENTATION.md](./API_DOCUMENTATION.md)*