# Create Gig

Creates a new gig with specified parameters. The gig will be created in a non-production state and will need to be funded before it can be started.

## Endpoint

`POST https://gigbot.xyz/api/gigs/create`

## Input Parameters

```typescript
{
  chain: "base",                               // Currently only Base network supported
  token_address: string,                       // ERC20 token contract address
  amount: number,                              // Reward amount in tokens (e.g., 10)
  start_time: string,                          // ISO datetime (e.g., "2025-03-01T15:00:00Z")
  duration: "1day" | "3days" | "1week" | "2weeks" | "3months" | "6months",
  gig_type: "mention" | "like" | "recast" | "boost" | "custom_bounty",
  how_to_earn?: string,                        // Required for `custom_bounty` else is Optional
  earning_criteria?: string,                   // Optional - will use default if not provided
  url?: string,                                // Required for like, recast, and boost tasks
  platform?: "farcaster" | "x"                 // Optional - defaults to "farcaster"
}
```

## Examples

### Mention Gig

```json
{
  "chain": "base",
  "token_address": "0x4ed4E862860beD51a9570b96d89aF5E1B0Efefed",
  "amount": 10,
  "start_time": "2025-03-01T15:00:00Z",
  "duration": "1week",
  "gig_type": "mention",
  "platform": "farcaster"
}
```

### Like Gig

```json
{
  "chain": "base",
  "token_address": "0x4ed4E862860beD51a9570b96d89aF5E1B0Efefed",
  "amount": 10,
  "start_time": "2025-03-01T15:00:00Z",
  "duration": "1week",
  "gig_type": "like",
  "url": "https://warpcast.com/gigbot.eth/0xf50c91de",
  "platform": "farcaster"
}
```

## Success Response (201)

```typescript
{
  gig_id: number,                    // Unique identifier for the created gig
  gig_wallet_address: string,        // Wallet address for reward distribution
  gig_token_address: string,         // Token contract address
  gig_chain_id: number,             // Chain ID (e.g., 8453 for Base)
  gig_balance: string,              // Reward amount in tokens
  platform_fee: string,             // Platform fee amount
  total_amount: string,             // Total including platform fee
  tx_data: string                   // Encoded transaction data
}
```

## Errors

### 400: Validation Errors

```json
{
  "error": "Validation failed",
  "details": ["Invalid chain", "URL required for like tasks"]
}
```

### 404: Resource Not Found

```json
{
  "error": "Chain not found"
}
```

### 500: Server Error

```json
{
  "error": "Failed to create gig"
}
```

## Important Notes

### Duration Options

- 1day: 24 hours (quick promotions)
- 3days: 72 hours (weekend events)
- 1week: 7 days (standard campaigns)
- 2weeks: 14 days (longer campaigns)
- 3months: 90 days (seasonal campaigns)
- 6months: 180 days (long-term campaigns)

### URL Format Requirements

- Farcaster: `https://warpcast.com/{username}/0x{hash}`
- X/Twitter: `https://twitter.com/{username}/status/{tweet_id}`

### Platform Support

- farcaster
- x
