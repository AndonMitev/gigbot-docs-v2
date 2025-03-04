# Start Gig

Endpoint: `POST /api/gigs/start`

Starts a previously created gig. The gig must be funded with the required amount of tokens before it can be started.

## Important Notes

1. Pre-start Checks:

   - Gig must exist and not be started
   - Wallet must have exact required funds
   - Uses Redis lock to prevent concurrent starts

2. Automatic Adjustments:

   - If start_time has passed, it will be updated to current time
   - End time adjusts accordingly

3. Post-start Effects:
   - Cannot modify gig after starting
   - Records gig transaction
   - Marks gig as production

## Request Body

```typescript
{
  gig_id: number; // ID of the gig to start
}
```

## Example

```json
{
  "gig_id": 123
}
```

## Responses

### Success (200)

```json
{
  "data": {
    "message": "Gig started successfully",
    "gig_id": 123
  }
}
```

### Error Responses

#### Not Funded (400)

```json
{
  "error": {
    "message": "Gig with id 123 is not funded",
    "details": [
      "Expected amount: 100 DEGEN",
      "Actual amount: 0 DEGEN",
      "Token Address: 0x4ed4...",
      "Chain: base",
      "Chain id: 8453",
      "Gig wallet address: 0x1234..."
    ]
  }
}
```

#### Already Started (400)

```json
{
  "error": "Gig is already live"
}
```

#### Not Found (404)

```json
{
  "error": "Gig with id 123 not found"
}
```

#### In Progress (409)

```json
{
  "error": "Gig with id 123 is currently being processed"
}
```

#### Server Error (500)

```json
{
  "error": "Failed to start gig"
}
```
