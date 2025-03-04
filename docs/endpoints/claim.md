# Claim API

## Overview

The Claim API allows users to fetch and claim their earned tokens from completed tasks.

## Endpoints

### 1. Fetch Claimable Tokens

`GET https://gigbot.xyz/api/claim`

#### Query Parameters

```typescript
{
  identifiers: string; // Comma-separated list of platform identifiers
}
```

#### Examples

```
/api/claim?identifiers=499205               // Single Farcaster FID
/api/claim?identifiers=499205,mitev_andon   // Farcaster FID + Twitter username
```

#### Success Response (200)

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
          "lock_timeout": "string (ISO date-time)",
          "token": {
            // Token details same as above
          },
          "gig": {
            "id": "number",
            "gig_address": "string",
            "chain": {
              "id": "number",
              "name": "string"
            },
            "compositeKey": "string"
          }
        }
      ],
      "gigs": [
        {
          "id": "number",
          "gig_address": "string",
          "chain": {
            "id": "number",
            "name": "string"
          },
          "compositeKey": "string"
        }
      ],
      "chain": {
        "id": "number",
        "name": "string"
      },
      "gigCount": "number"
    }
  },
  "summary": {
    "total_tokens": "number",
    "total_gigs": "number"
  }
}
```

### 2. Claim Tokens

`POST https://gigbot.xyz/api/claim`

#### Request Body

```json
{
  "url": "string" // URL of social media post
}
```

#### How to Claim

1. Create Post on Warpcast or X:

   ```
   // Option 1: With address
   Claim my rewards 0x77b4922fcc0fa745bcd7d76025e682cfffc9a149

   // Option 2: Without address (Farcaster only)
   Claim my rewards
   ```

2. Submit post URL:
   ```json
   {
     "url": "https://warpcast.com/user/0x123..."
   }
   ```

#### Success Response (200)

```json
{
  "transactions": {
    "failed": [
      {
        "token_address": "string",
        "token_symbol": "string",
        "amount": "number",
        "chain_name": "string",
        "status": "failed"
      }
    ],
    "successful": [
      {
        "token_address": "string",
        "token_symbol": "string",
        "amount": "number",
        "chain_name": "string",
        "tx_hash": "string",
        "status": "success"
      }
    ]
  },
  "summary": {
    "total_transactions": "number",
    "failed_count": "number",
    "success_count": "number",
    "recipient": "string"
  }
}
```

### Common Errors

#### 400 Bad Request

- Multiple "Claim my rewards" messages
- No "Claim my rewards" message
- Invalid Ethereum address
- Multiple Ethereum addresses

#### 404 Not Found

- Cast not found
- No tips found
- No verified address

#### 500 Server Error

- Internal server error while processing claim

## Notes

- Farcaster FIDs are numeric (e.g., "499205")
- Twitter usernames without @ (e.g., "mitev_andon")
- Farcaster URL format: `https://warpcast.com/{username}/0x{hash}`
- X/Twitter URL format: `https://twitter.com/{username}/status/{tweet_id}`
