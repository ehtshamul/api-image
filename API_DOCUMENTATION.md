# API Documentation

## Overview

This documentation covers the data structures and schemas for a plant e-commerce platform. The system manages users, products, posts, and comments through structured JSON data.

## Table of Contents

1. [Authentication & User Management](#authentication--user-management)
2. [Product Management](#product-management)
3. [Content Management](#content-management)
4. [Data Schemas](#data-schemas)
5. [API Endpoints](#api-endpoints)
6. [Usage Examples](#usage-examples)
7. [Error Handling](#error-handling)

## Authentication & User Management

### User Schema

```json
{
  "id": "integer",
  "name": "string",
  "email": "string",
  "password": "string"
}
```

**Field Descriptions:**
- `id`: Unique identifier for the user (auto-increment)
- `name`: Full name of the user
- `email`: User's email address (must be unique)
- `password`: User's password (should be hashed in production)

### User Management Endpoints

#### GET /api/users
Retrieve all users (excluding password field for security)

**Response:**
```json
{
  "users": [
    {
      "id": 1,
      "name": "Alice Johnson",
      "email": "alice.johnson@example.com"
    }
  ]
}
```

#### GET /api/users/{id}
Retrieve a specific user by ID

**Parameters:**
- `id` (path): User ID

**Response:**
```json
{
  "id": 1,
  "name": "Alice Johnson",
  "email": "alice.johnson@example.com"
}
```

#### POST /api/users
Create a new user

**Request Body:**
```json
{
  "name": "New User",
  "email": "newuser@example.com",
  "password": "securepassword123"
}
```

**Response:**
```json
{
  "id": 4,
  "name": "New User",
  "email": "newuser@example.com",
  "message": "User created successfully"
}
```

#### PUT /api/users/{id}
Update an existing user

**Parameters:**
- `id` (path): User ID

**Request Body:**
```json
{
  "name": "Updated Name",
  "email": "updated@example.com"
}
```

#### DELETE /api/users/{id}
Delete a user

**Parameters:**
- `id` (path): User ID

## Product Management

### Product Schema

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

**Field Descriptions:**
- `id`: Unique product identifier
- `name`: Product name
- `category`: Product category (e.g., "Indoor Plants", "Succulents", "Flowering Plants")
- `description`: Detailed product description
- `price`: Product price in decimal format
- `image`: URL to product image
- `inStock`: Availability status

### Product Categories

Available product categories:
- Indoor Plants
- Succulents
- Flowering Plants

### Product Management Endpoints

#### GET /api/products
Retrieve all products with optional filtering

**Query Parameters:**
- `category` (optional): Filter by category
- `inStock` (optional): Filter by availability (true/false)
- `minPrice` (optional): Minimum price filter
- `maxPrice` (optional): Maximum price filter

**Response:**
```json
{
  "products": [
    {
      "id": 1,
      "name": "Monstera Deliciosa",
      "category": "Indoor Plants",
      "description": "Beautiful split‑leaf houseplant perfect for bright, indirect light.",
      "price": 45.99,
      "image": "https://pixabay.com/photos/monstera-deliciosa-flora-nature-8426717/",
      "inStock": true
    }
  ],
  "total": 1,
  "page": 1,
  "limit": 10
}
```

#### GET /api/products/{id}
Retrieve a specific product by ID

**Parameters:**
- `id` (path): Product ID

**Response:**
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

#### POST /api/products
Create a new product

**Request Body:**
```json
{
  "name": "New Plant",
  "category": "Indoor Plants",
  "description": "A beautiful new plant",
  "price": 29.99,
  "image": "https://example.com/image.jpg",
  "inStock": true
}
```

#### PUT /api/products/{id}
Update an existing product

**Parameters:**
- `id` (path): Product ID

**Request Body:**
```json
{
  "name": "Updated Plant Name",
  "price": 39.99,
  "inStock": false
}
```

#### DELETE /api/products/{id}
Delete a product

**Parameters:**
- `id` (path): Product ID

## Content Management

### Post Schema

```json
{
  "userId": "integer",
  "id": "integer",
  "title": "string",
  "body": "string"
}
```

**Field Descriptions:**
- `userId`: ID of the user who created the post
- `id`: Unique post identifier
- `title`: Post title
- `body`: Post content

### Comment Schema

```json
{
  "postId": "integer",
  "id": "integer",
  "name": "string",
  "email": "string",
  "body": "string"
}
```

**Field Descriptions:**
- `postId`: ID of the post this comment belongs to
- `id`: Unique comment identifier
- `name`: Name of the commenter
- `email`: Email of the commenter
- `body`: Comment content

### Content Management Endpoints

#### GET /api/posts
Retrieve all posts

**Query Parameters:**
- `userId` (optional): Filter by user ID
- `page` (optional): Page number for pagination
- `limit` (optional): Number of posts per page

**Response:**
```json
{
  "posts": [
    {
      "userId": 1,
      "id": 1,
      "title": "My First Post",
      "body": "This is the body content of the first post."
    }
  ],
  "total": 1,
  "page": 1,
  "limit": 10
}
```

#### GET /api/posts/{id}
Retrieve a specific post by ID

**Parameters:**
- `id` (path): Post ID

**Response:**
```json
{
  "userId": 1,
  "id": 1,
  "title": "My First Post",
  "body": "This is the body content of the first post.",
  "comments": [
    {
      "postId": 1,
      "id": 1,
      "name": "Jane Doe",
      "email": "jane.doe@example.com",
      "body": "Great post! Thanks for sharing."
    }
  ]
}
```

#### POST /api/posts
Create a new post

**Request Body:**
```json
{
  "userId": 1,
  "title": "New Post Title",
  "body": "This is the content of the new post."
}
```

#### GET /api/posts/{id}/comments
Retrieve all comments for a specific post

**Parameters:**
- `id` (path): Post ID

**Response:**
```json
{
  "comments": [
    {
      "postId": 1,
      "id": 1,
      "name": "Jane Doe",
      "email": "jane.doe@example.com",
      "body": "Great post! Thanks for sharing."
    }
  ]
}
```

#### POST /api/posts/{id}/comments
Add a comment to a post

**Parameters:**
- `id` (path): Post ID

**Request Body:**
```json
{
  "name": "Commenter Name",
  "email": "commenter@example.com",
  "body": "This is a new comment."
}
```

## Data Schemas

### Complete Data Structure

The system consists of four main data collections:

1. **Users** (`auth.json`, `password.json`)
2. **Products** (`product_data.json`)
3. **Posts** (`post.json`)
4. **Comments** (`comments.json`)

### Relationships

- Users can create multiple posts (one-to-many)
- Posts can have multiple comments (one-to-many)
- Comments are linked to posts via `postId`
- Posts are linked to users via `userId`

## API Endpoints Summary

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/users` | Retrieve all users |
| GET | `/api/users/{id}` | Retrieve specific user |
| POST | `/api/users` | Create new user |
| PUT | `/api/users/{id}` | Update user |
| DELETE | `/api/users/{id}` | Delete user |
| GET | `/api/products` | Retrieve all products |
| GET | `/api/products/{id}` | Retrieve specific product |
| POST | `/api/products` | Create new product |
| PUT | `/api/products/{id}` | Update product |
| DELETE | `/api/products/{id}` | Delete product |
| GET | `/api/posts` | Retrieve all posts |
| GET | `/api/posts/{id}` | Retrieve specific post |
| POST | `/api/posts` | Create new post |
| GET | `/api/posts/{id}/comments` | Retrieve post comments |
| POST | `/api/posts/{id}/comments` | Add comment to post |

## Usage Examples

### JavaScript/Node.js Example

```javascript
// Fetch all products
async function getProducts() {
  try {
    const response = await fetch('/api/products');
    const data = await response.json();
    return data.products;
  } catch (error) {
    console.error('Error fetching products:', error);
  }
}

// Create a new user
async function createUser(userData) {
  try {
    const response = await fetch('/api/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(userData)
    });
    const result = await response.json();
    return result;
  } catch (error) {
    console.error('Error creating user:', error);
  }
}

// Add a comment to a post
async function addComment(postId, commentData) {
  try {
    const response = await fetch(`/api/posts/${postId}/comments`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(commentData)
    });
    const result = await response.json();
    return result;
  } catch (error) {
    console.error('Error adding comment:', error);
  }
}
```

### Python Example

```python
import requests
import json

# Fetch all products
def get_products():
    try:
        response = requests.get('/api/products')
        return response.json()['products']
    except requests.RequestException as e:
        print(f'Error fetching products: {e}')

# Create a new user
def create_user(user_data):
    try:
        response = requests.post('/api/users', 
                               json=user_data,
                               headers={'Content-Type': 'application/json'})
        return response.json()
    except requests.RequestException as e:
        print(f'Error creating user: {e}')

# Add a comment to a post
def add_comment(post_id, comment_data):
    try:
        response = requests.post(f'/api/posts/{post_id}/comments',
                               json=comment_data,
                               headers={'Content-Type': 'application/json'})
        return response.json()
    except requests.RequestException as e:
        print(f'Error adding comment: {e}')
```

### cURL Examples

```bash
# Get all products
curl -X GET "http://localhost:3000/api/products"

# Get products in a specific category
curl -X GET "http://localhost:3000/api/products?category=Indoor%20Plants"

# Create a new user
curl -X POST "http://localhost:3000/api/users" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john.doe@example.com",
    "password": "securepassword123"
  }'

# Add a comment to a post
curl -X POST "http://localhost:3000/api/posts/1/comments" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Commenter Name",
    "email": "commenter@example.com",
    "body": "This is a great post!"
  }'
```

## Error Handling

### Standard Error Response Format

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": "Additional error details if available"
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `VALIDATION_ERROR` | 400 | Request validation failed |
| `UNAUTHORIZED` | 401 | Authentication required |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `CONFLICT` | 409 | Resource conflict (e.g., duplicate email) |
| `INTERNAL_ERROR` | 500 | Internal server error |

### Error Handling Examples

```javascript
// Handle API errors
async function handleApiCall(apiCall) {
  try {
    const response = await apiCall();
    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.error.message);
    }
    return await response.json();
  } catch (error) {
    console.error('API Error:', error.message);
    throw error;
  }
}
```

## Security Considerations

1. **Password Handling**: Passwords should be hashed using bcrypt or similar algorithms
2. **Authentication**: Implement JWT tokens or session-based authentication
3. **Input Validation**: Validate all input data to prevent injection attacks
4. **Rate Limiting**: Implement rate limiting to prevent abuse
5. **CORS**: Configure CORS policies appropriately for web applications
6. **HTTPS**: Use HTTPS in production for secure data transmission

## Performance Considerations

1. **Pagination**: Implement pagination for large datasets
2. **Caching**: Use Redis or similar for caching frequently accessed data
3. **Database Indexing**: Index frequently queried fields (id, email, category)
4. **Image Optimization**: Optimize product images for web delivery
5. **API Versioning**: Consider API versioning for future compatibility

## Testing

### Sample Test Data

The JSON files in this workspace serve as sample data for testing:

- `auth.json` - User authentication data
- `password.json` - Alternative user data structure
- `product_data.json` - Product catalog
- `post.json` - Blog/content posts
- `comments.json` - User comments on posts

### Testing Endpoints

Use these sample data files to test your API implementation:

1. **User Tests**: Use data from `auth.json` to test user endpoints
2. **Product Tests**: Use data from `product_data.json` to test product endpoints
3. **Content Tests**: Use data from `post.json` and `comments.json` to test content endpoints

## Deployment

### Environment Variables

```bash
# Database configuration
DATABASE_URL=your_database_connection_string
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=plant_store
DATABASE_USER=username
DATABASE_PASSWORD=password

# API configuration
API_PORT=3000
API_HOST=0.0.0.0
NODE_ENV=production

# Security
JWT_SECRET=your_jwt_secret_key
BCRYPT_ROUNDS=12
```

### Docker Example

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

## Support

For questions or issues with this API:

1. Check the error logs for detailed error information
2. Verify request/response formats match the documented schemas
3. Ensure all required fields are provided in requests
4. Check authentication and authorization requirements

---

*This documentation covers the complete API structure for the plant e-commerce platform. All endpoints follow RESTful conventions and return JSON responses.*