# Create Gig

Endpoint: `POST /api/gigs/create`

Creates a new gig with specified parameters. The gig will be created in a non-production state and will need to be funded before it can be started.

## Important Notes

- Currently only supported on Base network
- Supported platforms: "farcaster" or "x" (default is "farcaster")
- For like and recast tasks, you MUST provide the url parameter
- For custom_bounty gig type, you MUST provide the how_to_earn parameter
- For custom_bounty gig type, you MUST provide the verifier parameter (Twitter username)
- URL format must match the platform:
  - Farcaster: https://warpcast.com/{username}/0x{hash}
  - X (Twitter): https://twitter.com/{username}/status/{tweet_id}
- Start time must be in ISO format (e.g., "2025-03-01T15:00:00Z")
- The amount must be a positive number

## Request Body

```typescript
{
  chain: "base",                               // Currently only Base network supported
  token_address: string,                       // ERC20 token contract address
  amount: number,                              // Reward amount in tokens (e.g., 10)
  start_time: string,                          // ISO datetime (e.g., "2025-03-01T15:00:00Z")
  duration: "1day" | "3days" | "1week" | "2weeks" | "3months" | "6months",
  gig_type: "mention" | "like" | "recast" | "boost" | "custom_bounty",
  how_to_earn?: string,                        // Required for custom_bounty else Optional
  earning_criteria?: string,                   // Optional - will use default if not provided
  url?: string,                                // Required for like, recast, and boost tasks
  platform?: "farcaster" | "x",                // Optional - defaults to "farcaster"
  verifier?: string                            // Required for custom_bounty (Twitter username)
}
```

## Duration Options

- 1day (24 hours) - Quick promotions
- 3days (72 hours) - Weekend events
- 1week (7 days) - Standard campaigns
- 2weeks (14 days) - Longer campaigns
- 3months (90 days) - Seasonal campaigns
- 6months (180 days) - Long-term campaigns

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

## Responses

### Success (201)

```json
{
  "gig_id": 123,
  "gig_wallet_address": "0x9876543210987654321098765432109876543210",
  "gig_token_address": "0x4ed4E862860beD51a9570b96d89aF5E1B0Efefed",
  "gig_chain_id": 8453,
  "gig_balance": "10.0",
  "platform_fee": "0.5",
  "total_amount": "10.5",
  "tx_data": "0xa9059cbb000000000000000000000000..."
}
```

### Error Responses

#### Validation Error (400)

```json
{
  "error": "Validation failed",
  "details": [
    "Invalid chain. Currently only \"base\" network is supported.",
    "URL is required for like and recast tasks",
    "Invalid platform. Must be either \"farcaster\" or \"x\""
  ]
}
```

#### Not Found (404)

```json
{
  "error": "Chain not found"
}
```

or

```json
{
  "error": "Token with address 0x1234... not found on chain base"
}
```

or

```json
{
  "error": "Task not found available tasks: mention, like, recast"
}
```

#### Server Error (500)

```json
{
  "error": "Failed to create gig"
}
```
