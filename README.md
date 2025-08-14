# Plant Store API Documentation

This repository contains comprehensive documentation for a plant e-commerce platform API, including data schemas, endpoints, and usage examples.

## 📚 Documentation Files

- **[API_DOCUMENTATION.md](./API_DOCUMENTATION.md)** - Complete, detailed API documentation
- **[API_REFERENCE.md](./API_REFERENCE.md)** - Quick reference guide for developers
- **[Plant_Store_API.postman_collection.json](./Plant_Store_API.postman_collection.json)** - Postman collection for testing

## 🏗️ Project Structure

The platform consists of four main data domains:

### 1. User Management
- User registration and authentication
- Profile management
- Access control

### 2. Product Catalog
- Plant inventory management
- Category-based organization
- Pricing and availability tracking

### 3. Content Management
- Blog posts and articles
- User-generated content
- Content relationships

### 4. Community Features
- User comments on posts
- Engagement tracking
- Social interactions

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ or Python 3.8+
- Database (PostgreSQL, MySQL, or MongoDB)
- API testing tool (Postman, Insomnia, or cURL)

### Quick Start
1. **Import Postman Collection**: Use the provided Postman collection for easy API testing
2. **Review Documentation**: Start with `API_REFERENCE.md` for quick overview
3. **Deep Dive**: Use `API_DOCUMENTATION.md` for comprehensive details
4. **Test Endpoints**: Use the sample data in JSON files for testing

## 📊 Data Models

### User Model
```json
{
  "id": "integer",
  "name": "string",
  "email": "string",
  "password": "string"
}
```

### Product Model
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

### Post Model
```json
{
  "userId": "integer",
  "id": "integer",
  "title": "string",
  "body": "string"
}
```

### Comment Model
```json
{
  "postId": "integer",
  "id": "integer",
  "name": "string",
  "email": "string",
  "body": "string"
}
```

## 🔐 Authentication

The API uses JWT (JSON Web Tokens) for authentication:

1. **Register**: Create a new user account
2. **Login**: Authenticate and receive JWT token
3. **Authorize**: Include token in `Authorization: Bearer <token>` header

## 📡 API Endpoints

### Public Endpoints (No Auth Required)
- `GET /api/products` - Browse products
- `GET /api/products/{id}` - View product details
- `GET /api/posts` - Read blog posts
- `GET /api/posts/{id}` - View post content
- `GET /api/posts/{id}/comments` - Read post comments
- `POST /api/posts/{id}/comments` - Add comments
- `POST /api/users` - User registration
- `POST /api/auth/login` - User login

### Protected Endpoints (Auth Required)
- `GET /api/users` - View user profiles
- `GET /api/users/{id}` - View specific user
- `PUT /api/users/{id}` - Update user profile
- `DELETE /api/users/{id}` - Delete user account
- `POST /api/products` - Add new products
- `PUT /api/products/{id}` - Update products
- `DELETE /api/products/{id}` - Remove products
- `POST /api/posts` - Create new posts

## 🧪 Testing

### Using Postman
1. Import `Plant_Store_API.postman_collection.json`
2. Set environment variables:
   - `base_url`: `http://localhost:3000/api`
   - `auth_token`: Your JWT token after login
3. Test endpoints in order (register → login → protected endpoints)

### Using cURL
```bash
# Get all products
curl -X GET "http://localhost:3000/api/products"

# Create user
curl -X POST "http://localhost:3000/api/users" \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@example.com","password":"password123"}'
```

### Sample Data
The repository includes sample data files for testing:
- `auth.json` - User accounts
- `product_data.json` - Plant inventory
- `post.json` - Blog posts
- `comments.json` - User comments

## 🔧 Development

### Environment Setup
```bash
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/plant_store

# API Configuration
API_PORT=3000
API_HOST=0.0.0.0
NODE_ENV=development

# Security
JWT_SECRET=your_secret_key
BCRYPT_ROUNDS=12
```

### Code Examples

#### JavaScript/Node.js
```javascript
// Fetch products
const response = await fetch('/api/products');
const products = await response.json();

// Create user
const userResponse = await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(userData)
});
```

#### Python
```python
import requests

# Get products
response = requests.get('/api/products')
products = response.json()

# Create user
user_response = requests.post('/api/users', json=user_data)
```

## 📈 Features

### Product Management
- **Categories**: Indoor Plants, Succulents, Flowering Plants
- **Filtering**: By category, price, availability
- **Search**: Full-text search capabilities
- **Pagination**: Efficient data loading

### User Experience
- **Public Access**: Browse products and read content without registration
- **User Accounts**: Personalized experience with user profiles
- **Content Creation**: Users can create posts and comments
- **Social Features**: Community engagement through comments

### Performance
- **Caching**: Redis-based caching for frequently accessed data
- **Pagination**: Efficient data loading for large datasets
- **Image Optimization**: Optimized product images
- **Rate Limiting**: API abuse prevention

## 🛡️ Security

- **Password Hashing**: Bcrypt with configurable rounds
- **JWT Authentication**: Secure token-based authentication
- **Input Validation**: Comprehensive request validation
- **Rate Limiting**: Protection against API abuse
- **CORS Configuration**: Proper cross-origin resource sharing
- **HTTPS Enforcement**: Secure data transmission

## 🚀 Deployment

### Docker
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

### Environment Variables
```bash
# Production settings
NODE_ENV=production
API_PORT=3000
DATABASE_URL=your_production_db_url
JWT_SECRET=your_production_secret
```

## 📝 API Versioning

Current version: `v1`

Access specific versions via URL:
```
http://localhost:3000/api/v1/users
http://localhost:3000/api/v2/users
```

## 🤝 Contributing

1. **Fork** the repository
2. **Create** a feature branch
3. **Update** documentation as needed
4. **Test** all endpoints
5. **Submit** a pull request

## 📞 Support

For questions or issues:

1. Check the documentation files
2. Review error logs
3. Test with Postman collection
4. Verify request/response formats
5. Check authentication requirements

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**Happy coding! 🌱**

*This documentation provides everything you need to understand, implement, and test the Plant Store API platform.*