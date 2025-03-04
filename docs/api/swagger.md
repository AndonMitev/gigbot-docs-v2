# Interactive API Documentation

Our API is documented using OpenAPI (Swagger) specification, providing an interactive way to explore and test the API endpoints.

## Swagger UI

You can access our interactive API documentation at:
[https://www.gigbot.xyz/api-doc](https://www.gigbot.xyz/api-doc)

## Features

The Swagger UI provides:

- Interactive endpoint testing
- Request/response examples
- Schema definitions
- Authentication information
- Real-time API responses

## How to Use

1. Visit the [Swagger documentation](https://www.gigbot.xyz/api-doc)
2. Authenticate using your API key:
   - Click the "Authorize" button
   - Enter your API key
   - Click "Authorize"
3. Explore available endpoints:
   - Expand endpoint sections
   - View request/response schemas
   - Try out endpoints directly
4. Test endpoints:
   - Fill in required parameters
   - Click "Execute"
   - View responses

## Available Sections

- **Gigs API**
  - Create gigs
  - Start gigs
  - Get gig types
- **Claims API**
  - Get claimable tokens
  - Claim tokens

## Authentication

All API endpoints require authentication. In the Swagger UI:

1. Click "Authorize" button
2. Enter your API key in the format: `Bearer YOUR_API_KEY`
3. Click "Authorize"

## Response Codes

- 200: Successful operation
- 201: Resource created
- 400: Bad request
- 401: Unauthorized
- 404: Not found
- 500: Server error

## Need Help?

If you need assistance:

1. Check the example requests/responses
2. Review the schema definitions
3. Contact our support team if issues persist
