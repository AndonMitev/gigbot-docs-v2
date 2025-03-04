# Gigs API

## Overview

The Gigs API allows you to create, manage, and start tasks (gigs) for your community engagement campaigns.

## Endpoints

### 1. Create Gig

`POST https://gigbot.xyz/api/gigs/create`

Creates a new gig with specified parameters. The gig will be created in a non-production state and will need to be funded before it can be started.

#### Input Parameters

```typescript
{
  chain: "base",                               // Currently only Base network supported
  token_address: string,                       // ERC20 token contract address
  amount: number,                              // Reward amount in tokens (e.g., 10)
  start_time: string,                          // ISO datetime (e.g., "2025-03-01T15:00:00Z")
  duration: "1day" | "3days" | "1week" | "2weeks" | "3months" | "6months",
  gig_type: "mention" | "like" | "recast" | "boost" | "custom_bounty",
  how_to_earn?: string,                        // Required for `custom_bounty` else is Optional - will use default if not provided
  earning_criteria?: string,                   // Optional - will use default if not provided
  url?: string,                                // Required for like, recast, and boost tasks
  platform?: "farcaster" | "x"                 // Optional - defaults to "farcaster"
}
```

#### Examples

##### Mention Gig

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

##### Like Gig

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

#### Success Response (201)

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

#### Errors

- 400: Validation errors
  ```json
  {
    "error": "Validation failed",
    "details": ["Invalid chain", "URL required for like tasks"]
  }
  ```
- 404: Resource not found
  ```json
  {
    "error": "Chain not found"
  }
  ```
- 500: Server error
  ```json
  {
    "error": "Failed to create gig"
  }
  ```

### 2. Start Gig

`POST https://gigbot.xyz/api/gigs/start`

Starts a previously created gig after verifying funding and requirements.

#### Input Parameters

```typescript
{
  gig_id: number; // ID of the gig to start
}
```

#### Example

```json
{
  "gig_id": 123
}
```

#### Success Response (200)

```json
{
  "data": {
    "message": "Gig started successfully",
    "gig_id": 123
  }
}
```

#### Errors

##### Not Funded (400)

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

##### Already Started (400)

```json
{
  "error": "Gig is already live"
}
```

##### Not Found (404)

```json
{
  "error": "Gig with id 123 not found"
}
```

##### In Progress (409)

```json
{
  "error": "Gig with id 123 is currently being processed"
}
```

### 3. Get Gig Types

`GET https://gigbot.xyz/api/gigs/types`

Retrieves all available gig types for filtering gigs.

#### Success Response (200)

```json
{
  "data": ["mention", "like", "recast", "boost", "reply", "follow"]
}
```

#### Error (500)

```json
{
  "error": "Failed to fetch gig types"
}
```

## Important Notes

1. Duration Options:

   - 1day: 24 hours (quick promotions)
   - 3days: 72 hours (weekend events)
   - 1week: 7 days (standard campaigns)
   - 2weeks: 14 days (longer campaigns)
   - 3months: 90 days (seasonal campaigns)
   - 6months: 180 days (long-term campaigns)

2. URL Format Requirements:

   - Farcaster: `https://warpcast.com/{username}/0x{hash}`
   - X (Twitter): `https://twitter.com/{username}/status/{tweet_id}`

3. Platform Support:

   - farcaster
   - x

4. Pre-start Checks:

   - Gig must exist and not be started
   - Wallet must have exact required funds
   - Uses Redis lock to prevent concurrent starts

5. Post-start Effects:
   - Cannot modify gig after starting
   - Records gig transaction
   - Marks gig as production
