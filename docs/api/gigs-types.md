# Get Gig Types

Endpoint: `GET /api/gigs/types`

Retrieves a list of all available gig types (task types) that can be used for creating gigs.

## Response

### Success (200)

```json
{
  "data": ["mention", "like", "recast", "boost", "reply", "follow"]
}
```

### Error Response

#### Server Error (500)

```json
{
  "error": "Failed to fetch gig types"
}
```
