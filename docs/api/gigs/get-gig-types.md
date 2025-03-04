# Get Gig Types

Retrieve all available gig types for filtering gigs.

## Endpoint

`GET https://gigbot.xyz/api/gigs/types`

## Response

### Success (200)

```json
{
  "data": ["mention", "like", "recast", "boost", "reply", "follow"]
}
```

### Error (500)

```json
{
  "error": "Failed to fetch gig types"
}
```

## Notes

- Returns all supported task types that can be used when creating gigs
- No parameters required
- Task types are used for filtering and creating gigs
