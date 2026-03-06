# Using the Complete API Documentation Files

This guide explains how to use the different API documentation formats provided.

## Files Included

### 1. **API_COMPLETE_REFERENCE.md** (Markdown)
A comprehensive, human-readable reference document with all 12 endpoints.

**Best for**: 
- Quick lookup of endpoint details
- Understanding request/response formats
- Learning about error codes and HTTP statuses
- Reading on GitHub/documentation sites

**How to use**:
- Open in any markdown viewer or text editor
- Search for endpoint names
- Copy cURL examples directly

---

### 2. **openapi.yaml** (OpenAPI 3.0 Specification)
A machine-readable API specification in YAML format.

**Best for**:
- Generating interactive API documentation
- Importing into API testing tools
- Generating SDKs automatically
- API gateway configuration

**How to use**:

#### Option A: View in Swagger UI (Online)
```
1. Go to https://editor.swagger.io/
2. Click "File" → "Import URL"
3. Paste: https://your-domain.com/openapi.yaml
4. View interactive documentation with try-it features
```

#### Option B: Use with SwaggerUI locally
```bash
# Install swagger-ui locally
npm install -g swagger-ui-dist

# Or use Docker
docker run -p 8080:8080 \
  -e SWAGGER_JSON=/data/openapi.yaml \
  -v /path/to/openapi.yaml:/data/openapi.yaml \
  swaggerapi/swagger-ui
```

#### Option C: Import into API Gateway
- **AWS API Gateway**: Upload openapi.yaml directly
- **Kong**: Use the specification for API configuration
- **Apigee**: Import as API proxy specification

---

### 3. **Postman_Collection.json** (Postman Format)
Ready-to-use collection for testing the API in Postman.

**Best for**:
- Testing API endpoints
- Automating API testing
- Team collaboration with shared collections
- Pre-configured requests with authentication

**How to use**:

#### Method 1: Import into Postman Desktop App
```
1. Open Postman
2. Click "Import" button (top left)
3. Select "Upload Files"
4. Choose Postman_Collection.json
5. Click "Import"
```

#### Method 2: Import from URL
```
1. In Postman, click "Import"
2. Select "Link" tab
3. Paste: https://your-domain.com/Postman_Collection.json
4. Click "Continue" and "Import"
```

#### Method 3: Using Postman CLI
```bash
postman collection run ./Postman_Collection.json \
  -e postman_environment.json
```

**Setting up Environment Variables**:
```
1. In Postman, click "Environments" (left sidebar)
2. Click "+" to create new environment
3. Add variables:
   - base_url: https://api.varansinagar.gov.in/api
   - jwt_token: (empty - filled after login)
4. Select environment in top-right dropdown
```

**Testing the Collection**:
1. Run "Send OTP" request
2. Check the response
3. Run "Verify OTP" request with the OTP
4. JWT token is automatically saved to environment
5. Run any authenticated endpoint - token is automatically included

---

## File Comparison

| Feature | Markdown | OpenAPI | Postman |
|---------|----------|---------|---------|
| Human Readable | ✓ | △ | ✓ |
| Machine Readable | ✗ | ✓ | ✓ |
| Interactive | ✗ | ✓ | ✓ |
| Test Requests | ✗ | ✓ | ✓ |
| Generate Docs | ✓ | ✓ | ✗ |
| Generate Code | ✗ | ✓ | ✓ |
| Authentication Tests | ✗ | △ | ✓ |
| Sharing | ✓ | ✓ | ✓ |

---

## Recommended Setup

### For Documentation
1. **Primary**: API_COMPLETE_REFERENCE.md
2. **Backup**: openapi.yaml (for Swagger UI)

### For Testing
1. Use Postman_Collection.json
2. Set up environment variables
3. Run requests in order for proper authentication flow

### For Development Teams
```
1. Share openapi.yaml in code repository
2. Generate client SDKs using OpenAPI tools
3. Share Postman collection for QA testing
4. Host Markdown docs on internal wiki
```

### For Public API Documentation
```
1. Use openapi.yaml with Swagger UI
2. Deploy on documentation site
3. Include API_COMPLETE_REFERENCE.md as guide
4. Provide Postman collection download link
```

---

## How to Update Files

### When API Changes
1. **Update markdown first**: Edit API_COMPLETE_REFERENCE.md
2. **Update OpenAPI**: Edit openapi.yaml
3. **Update Postman**: Download collection, edit, re-upload

### Keeping Files Synchronized
Use a tool like:
- **Swagger Codegen**: Generate from OpenAPI
- **Prism**: Mock API from OpenAPI
- **API Sync**: Keep multiple formats in sync

---

## Using with Development

### Generate Client Libraries
```bash
# Generate from OpenAPI
openapi-generator-cli generate \
  -i openapi.yaml \
  -g javascript \
  -o ./generated-client

# Supports: nodejs, python, java, go, ruby, etc.
```

### Mock API Server
```bash
# Using Prism
prism mock openapi.yaml -p 4010

# API will be available at http://localhost:4010
```

### Documentation Generation
```bash
# Generate beautiful docs with ReDoc
redoc-cli bundle openapi.yaml -o index.html

# Or Swagger UI (self-hosted)
docker run -p 8080:8080 \
  -e SWAGGER_JSON=/data/openapi.yaml \
  -v $(pwd)/openapi.yaml:/data/openapi.yaml \
  swaggerapi/swagger-ui
```

---

## Integration Checklist

- [ ] Download all three files to your project
- [ ] Set up Postman environment with base_url
- [ ] Test each endpoint in Postman
- [ ] Review API_COMPLETE_REFERENCE.md for details
- [ ] Set up OpenAPI in documentation portal
- [ ] Share Postman collection with team
- [ ] Generate client SDKs if needed
- [ ] Set up automated API testing

---

## Support & Issues

**Documentation Errors**: api-support@varansinagar.gov.in
**API Issues**: api-support@varansinagar.gov.in
**Postman Issues**: Check Postman documentation

---

## Next Steps

1. **Start Testing**: Import Postman collection and test
2. **Read Details**: Check API_COMPLETE_REFERENCE.md for specifics
3. **Integrate**: Use OpenAPI spec for development
4. **Deploy**: Host documentation using Swagger/ReDoc
5. **Monitor**: Use Postman for regression testing

---

**Version**: 1.0  
**Last Updated**: 6 March 2026  
**Format Compatibility**: OpenAPI 3.0, Postman 2.1
