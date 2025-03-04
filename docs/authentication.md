# Authentication Guide

## Getting an API Key

To use the GigBot API, you'll need an API key. You can obtain one by:

1. Creating an account at [GigBot](https://www.gigbot.xyz)
2. Navigating to your Dashboard
3. Going to the API Settings section
4. Generating a new API key

## Using Your API Key

Include your API key in all API requests using the Authorization header:

```http
Authorization: Bearer YOUR_API_KEY
```

## API Key Security

To keep your API key secure:

- Never share your API key publicly
- Don't commit it to version control
- Rotate keys periodically
- Use environment variables in your applications

## Example Request

Here's an example of how to make an authenticated request:

```javascript
const response = await fetch('https://www.gigbot.xyz/api/tasks', {
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json'
  }
});
```

## Rate Limiting

Each API key has its own rate limits:

- 100 requests per minute
- 1000 requests per hour

When you exceed these limits, you'll receive a 429 Too Many Requests response.

## Key Permissions

API keys can have different permission levels:

- Read-only
- Write
- Admin

Make sure to request the appropriate permissions when generating your key.
