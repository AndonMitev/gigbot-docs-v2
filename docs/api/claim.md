# Claim Tokens API

## Get Claimable Tokens

Endpoint: `GET /api/claim`

Fetch claimable tokens for a given account. Identifiers should be provided as a comma-separated list.

### Platform Identifier Formats

- Farcaster: Numeric FID (e.g., "499205")
- Twitter: Username without @ (e.g., "mitev_andon")

### Example URLs

- Single platform: `/api/claim?identifiers=499205`
- Multiple platforms: `/api/claim?identifiers=499205,mitev_andon`

### Success Response (200)

```json
{
  "data": {
    "0x4ed4E862860beD51a9570b96d89aF5E1B0Efefed": {
      "token": {
        "id": 1,
        "address": "0x4ed4E862860beD51a9570b96d89aF5E1B0Efefed",
        "chain_id": 8453,
        "decimals": 18,
        "image_url": "https://example.com/token.png",
        "name": "DEGEN",
        "symbol": "DEGEN",
        "is_native": false,
        "coingecko_id": "degen"
      },
      "totalValue": 100.5,
      "tips": [
        {
          "id": 1,
          "created_at": "2024-03-01T15:00:00Z",
          "value": 50.25,
          "platform_user_id": "499205",
          "username": "user1",
          "user_address": "0x123...",
          "status": "pending",
          "platform": "farcaster"
        }
      ],
      "gigs": [
        {
          "id": 1,
          "gig_address": "0x456...",
          "chain": {
            "id": 1,
            "name": "base"
          }
        }
      ],
      "chain": {
        "id": 1,
        "name": "base"
      },
      "gigCount": 1
    }
  },
  "summary": {
    "total_tokens": 1,
    "total_gigs": 1
  }
}
```

### Error Responses

#### Invalid Request (400)

```json
{
  "error": "Invalid identifiers"
}
```

#### Server Error (500)

```json
{
  "error": "Failed to fetch unclaimed tips"
}
```

## Claim Tokens

Endpoint: `POST /api/claim`

Claim tokens using a social media post.

### How to Claim

1. Create a Post

   - Go to a supported platform (currently Warpcast and X)
   - Create a post with EITHER of these message formats:
     ```
     Claim my rewards 0x77b4922fcc0fa745bcd7d76025e682cfffc9a149
     ```
     or (Supported only on Farcaster):
     ```
     Claim my rewards
     ```
   - If providing an address, make sure it's a valid Ethereum address
   - The message must contain exactly one "Claim my rewards" phrase
   - If no address is provided, the system will look up your verified address

2. Submit the Post URL
   ```json
   {
     "url": "https://warpcast.com/jxjxjxj/0x629ef38f"
   }
   ```

### Success Response (200)

#### With Successful Claims

```json
{
  "transactions": {
    "failed": [
      {
        "token_address": "0x4ed4e862860bed51a9570b96d89af5e1b0efefed",
        "token_symbol": "DEGEN",
        "amount": 15.73,
        "chain_name": "Base",
        "status": "failed"
      }
    ],
    "successful": [
      {
        "token_address": "0x22af33fe49fd1fa80c7149773dde5890d3c76f3b",
        "token_symbol": "BNKR",
        "amount": 0.0001,
        "chain_name": "Base",
        "tx_hash": "0xdc60b37969b7540c0e103567b5aa1348aca148047894ba073a1716a777e57e39",
        "status": "success"
      }
    ]
  },
  "summary": {
    "total_transactions": 2,
    "failed_count": 1,
    "success_count": 1,
    "recipient": "0x77b4922fcc0fa745bcd7d76025e682cfffc9a149"
  }
}
```

#### No Pending Claims

```json
{
  "transactions": {
    "failed": [],
    "successful": []
  },
  "summary": {
    "total_transactions": 0,
    "failed_count": 0,
    "success_count": 0,
    "recipient": "0x77b4922fcc0fa745bcd7d76025e682cfffc9a149"
  }
}
```

### Error Responses

#### Invalid Request (400)

```json
{
  "error": "Message must contain \"claim my rewards\" with an optional Ethereum address"
}
```

or

```json
{
  "error": "Invalid Ethereum address provided"
}
```

or

```json
{
  "error": "Multiple Ethereum addresses found in the cast"
}
```

or

```json
{
  "error": "No verified address found"
}
```

#### Not Found (404)

```json
{
  "error": "Cast not found"
}
```

or

```json
{
  "error": "No tips found"
}
```

#### Server Error (500)

```json
{
  "error": "Internal server error"
}
```
