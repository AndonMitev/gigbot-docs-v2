# Claim Tokens

Submit a claim for tokens by providing a social media post URL.

## Endpoint

`POST https://gigbot.xyz/api/claim`

## Request Body

```json
{
  "url": "string" // URL of social media post
}
```

## How to Claim

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

## Success Response (200)

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

## Common Errors

### 400 Bad Request

- Multiple "Claim my rewards" messages
- No "Claim my rewards" message
- Invalid Ethereum address
- Multiple Ethereum addresses

### 404 Not Found

- Cast not found
- No tips found
- No verified address

### 500 Server Error

- Internal server error while processing claim

## URL Format Requirements

- Farcaster: `https://warpcast.com/{username}/0x{hash}`
- X/Twitter: `https://twitter.com/{username}/status/{tweet_id}`
