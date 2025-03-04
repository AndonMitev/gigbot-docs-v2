# GigBot API Documentation Testish

Welcome to the GigBot MXM API documentation. This documentation provides detailed information about the GigBot API endpoints, their usage, and examples.

## Overview

GigBot is a platform that enables users to create and manage social media engagement gigs with token rewards. The API provides endpoints for:

- Creating and managing gigs
- Starting gigs
- Retrieving gig types
- Claiming rewards

## Base URL

All API endpoints are accessible at:

```
https://gigbot.xyz/api
```

## Available Endpoints

### Gigs

- [Create Gig](/api/gigs-create.md) - Create a new gig
- [Start Gig](/api/gigs-start.md) - Start an existing gig
- [Get Gig Types](/api/gigs-types.md) - Get available gig types

### Claims

- [Claim Tokens](/api/claim.md) - View and claim earned tokens

## Response Format

All responses are returned in JSON format. Successful responses typically include a `data` field containing the requested information. Error responses include an `error` field with a description of what went wrong.

### Success Response Format

```json
{
  "data": {
    // Response data
  }
}
```

### Error Response Format

```json
{
  "error": "Error message",
  "details": [
    // Optional array of detailed error messages
  ]
}
```

## Rate Limiting

The API implements rate limiting to ensure fair usage:

- 100 requests per minute per IP address

## Support

If you need help or have questions about the API, please:

1. Check the detailed documentation for each endpoint
2. Join our [Discord community](https://discord.gg/gigbot)
3. Contact us at support@gigbot.xyz
