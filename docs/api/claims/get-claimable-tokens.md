# Get Claimable Tokens

Get a list of tokens that can be claimed by a user.

## Endpoint

`GET https://gigbot.xyz/api/claim`

## Query Parameters

```typescript
{
  identifiers: string; // Comma-separated list of platform identifiers
}
```

## Examples

```bash
# Single Farcaster FID
/api/claim?identifiers=499205

# Farcaster FID + Twitter username
/api/claim?identifiers=499205,mitev_andon
```

## Success Response (200)

```json
{
  "data": {
    "[token_address]": {
      "token": {
        "id": "number",
        "created_at": "string (ISO date-time)",
        "address": "string (contract address)",
        "chain_id": "number",
        "decimals": "number",
        "image_url": "string",
        "name": "string",
        "symbol": "string",
        "is_native": "boolean",
        "coingecko_id": "string | null"
      },
      "totalValue": "number",
      "tips": [
        {
          "id": "number",
          "created_at": "string (ISO date-time)",
          "token_id": "number",
          "value": "number (float)",
          "tx_hash": "string | null",
          "platform_user_id": "string",
          "username": "string | null",
          "user_address": "string",
          "claimed_at": "string (ISO date-time)",
          "status": "pending | claimed",
          "gig_id": "number",
          "platform": "farcaster | twitter",
          "claim_nonce": "string | null",
          "claim_started_at": "string (ISO date-time)",
          "lock_timeout": "string (ISO date-time)"
        }
      ],
      "gigCount": "number"
    }
  },
  "summary": {
    "total_tokens": "number",
    "total_gigs": "number"
  }
}
```

## Notes

- Farcaster FIDs are numeric (e.g., "499205")
- Twitter usernames without @ (e.g., "mitev_andon")
