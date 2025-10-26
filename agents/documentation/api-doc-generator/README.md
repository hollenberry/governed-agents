# API Documentation Generator Agent

An intelligent agent that generates comprehensive API documentation from your code, including OpenAPI/Swagger specifications, examples, and usage guides.

## Description

The API Documentation Generator analyzes your API code and creates professional, comprehensive documentation including endpoint descriptions, request/response schemas, code examples, and OpenAPI specifications. It helps maintain accurate, up-to-date API documentation with minimal manual effort.

## Use Cases

This agent is useful for:

- **API Reference Generation**: Create complete API reference documentation
- **OpenAPI/Swagger Specs**: Generate standard API specifications
- **Developer Onboarding**: Help new developers understand APIs
- **Documentation Updates**: Keep docs in sync with code changes
- **Multi-Format Output**: Generate docs in various formats (Markdown, HTML, YAML)

## Prerequisites

Before using this agent, ensure:

- [ ] API code has comments or docstrings
- [ ] Endpoints follow consistent patterns
- [ ] You have read access to the codebase

## Installation

To use this agent in your repository:

1. Copy the agent directory to your repository:
   ```bash
   cp -r agents/documentation/api-doc-generator .github/agents/
   ```

2. No additional configuration needed!

## Supported API Types

- ✅ REST APIs
- ✅ GraphQL APIs (basic support)
- ✅ HTTP endpoints
- ⚠️ WebSocket APIs (limited support)
- ⚠️ gRPC (experimental)

## Usage

### Generate Full API Documentation

```
Generate complete API documentation for this project
```

### Generate OpenAPI Specification

```
Create an OpenAPI 3.0 specification for all endpoints in src/api/
```

### Document Specific Endpoints

```
Generate documentation for the user authentication endpoints
```

### Update Existing Documentation

```
Update the API docs to reflect the changes in the latest commit
```

### Example 1: REST API Documentation

**Input:**
```
Generate API documentation for the endpoints in src/controllers/users.js
```

**Output:**
```markdown
# User API Documentation

## Overview

The User API provides endpoints for managing user accounts, authentication, and profiles.

Base URL: `https://api.example.com/v1`

## Authentication

All endpoints require Bearer token authentication:

```
Authorization: Bearer <your-token>
```

## Endpoints

### Create User

**POST** `/users`

Creates a new user account.

**Request Headers:**
- `Content-Type: application/json`
- `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "email": "string (required) - User email address",
  "password": "string (required) - Min 8 characters",
  "name": "string (required) - Full name",
  "role": "string (optional) - user|admin, default: user"
}
```

**Response:** `201 Created`
```json
{
  "id": "string - User ID",
  "email": "string - User email",
  "name": "string - User name",
  "role": "string - User role",
  "createdAt": "string - ISO 8601 timestamp"
}
```

**Example Request:**
```bash
curl -X POST https://api.example.com/v1/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer eyJ..." \
  -d '{
    "email": "john@example.com",
    "password": "SecurePass123",
    "name": "John Doe"
  }'
```

**Example Response:**
```json
{
  "id": "usr_123abc",
  "email": "john@example.com",
  "name": "John Doe",
  "role": "user",
  "createdAt": "2025-10-26T10:30:00Z"
}
```

**Error Responses:**

- `400 Bad Request`: Invalid input data
  ```json
  {
    "error": "Validation failed",
    "details": ["Email already exists"]
  }
  ```

- `401 Unauthorized`: Missing or invalid authentication
- `500 Internal Server Error`: Server error

---

### Get User

**GET** `/users/{id}`

Retrieves user information by ID.

[Additional endpoint documentation...]
```

### Example 2: OpenAPI Specification

**Input:**
```
Create an OpenAPI specification for all endpoints
```

**Output:** (openapi.yml)
```yaml
openapi: 3.0.0
info:
  title: Example API
  description: API for managing users and resources
  version: 1.0.0
  contact:
    email: api@example.com

servers:
  - url: https://api.example.com/v1
    description: Production server
  - url: https://staging-api.example.com/v1
    description: Staging server

paths:
  /users:
    post:
      summary: Create a new user
      tags:
        - Users
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /users/{id}:
    get:
      summary: Get user by ID
      tags:
        - Users
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: User found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
        email:
          type: string
          format: email
        name:
          type: string
        role:
          type: string
          enum: [user, admin]
        createdAt:
          type: string
          format: date-time

    CreateUserRequest:
      type: object
      required:
        - email
        - password
        - name
      properties:
        email:
          type: string
          format: email
        password:
          type: string
          minLength: 8
        name:
          type: string
        role:
          type: string
          enum: [user, admin]
          default: user

    Error:
      type: object
      properties:
        error:
          type: string
        details:
          type: array
          items:
            type: string

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

## Generated Documentation Includes

- ✅ Endpoint descriptions
- ✅ Request/response schemas
- ✅ HTTP methods and paths
- ✅ Headers and parameters
- ✅ Status codes
- ✅ Error responses
- ✅ Authentication details
- ✅ Code examples (curl, JavaScript, Python)
- ✅ OpenAPI/Swagger specs
- ✅ Rate limiting info
- ✅ Versioning details

## Output Formats

- **Markdown**: Human-readable documentation
- **OpenAPI/Swagger**: Machine-readable API spec
- **HTML**: Static documentation site
- **JSON Schema**: Data model definitions

## Limitations

- **Code Quality Dependent**: Better documented code → better output
- **Pattern Recognition**: Works best with consistent API patterns
- **No Live Testing**: Cannot test actual API endpoints
- **Manual Review**: May require manual refinement

## Troubleshooting

### Issue: Incomplete Documentation

**Solution:** Ensure code has adequate comments:
```javascript
/**
 * Creates a new user account
 * @param {Object} userData - User data
 * @param {string} userData.email - User email
 * @param {string} userData.password - User password
 * @returns {Promise<User>} Created user
 */
```

### Issue: Missing Endpoints

**Solution:** Be specific about which files to analyze:
```
Generate docs for all files in src/api/v1/
```

## Performance Considerations

- Small API (< 20 endpoints): 1-2 minutes
- Medium API (20-100 endpoints): 3-5 minutes
- Large API (> 100 endpoints): 5-10 minutes

## Integration

### CI/CD Pipeline

```yaml
# .github/workflows/docs.yml
name: Update API Docs
on:
  push:
    branches: [main]
jobs:
  docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Generate API Docs
        run: |
          # Invoke api-doc-generator agent
```

## Best Practices

- Keep code comments up-to-date
- Use consistent naming conventions
- Follow REST/API design patterns
- Version your APIs properly
- Review generated docs before publishing

## Security Notes

This agent:
- Only requires read access to code
- Does not expose sensitive data
- Does not call actual API endpoints

## Version History

See [CHANGELOG.md](./CHANGELOG.md)

## Support

- **Questions**: #custom-agents Slack channel
- **Email**: agents-support@company.com

## License

Proprietary - Internal use only

## Author

**Team**: Developer Experience  
**Email**: docs@company.com  
**Last Updated**: 2025-10-26
