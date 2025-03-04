# Common Use Cases

## Social Media Engagement Campaign

### Scenario

You want to run a social media campaign where users can earn tokens for engaging with your content.

### Implementation

1. Create a Mention Gig:

```typescript
const mentionGig = {
  chain: "base",
  token_address: "0x4ed4E862860beD51a9570b96d89aF5E1B0Efefed",
  amount: 10,
  start_time: "2025-03-01T15:00:00Z",
  duration: "1week",
  gig_type: "mention",
  platform: "farcaster"
};

const response = await fetch('https://gigbot.xyz/api/gigs/create', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(mentionGig)
});
```

2. Fund the Gig:

   - Send tokens to the returned `gig_wallet_address`
   - Amount should match `total_amount` from response

3. Start the Gig:

```typescript
await fetch('https://gigbot.xyz/api/gigs/start', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ gig_id: response.gig_id })
});
```

## Automated Reward Distribution

### Scenario

You want to automatically distribute rewards to users who complete tasks.

### Implementation

1. Set up ElizaOS Plugin:

```typescript
import { GigBotClientInterface } from "@elizaos/gigbot";

const gigbotPlugin = {
    name: "gigbot",
    description: "GigBot client",
    clients: [GigBotClientInterface],
};

runtime.registerPlugin(gigbotPlugin);
```

2. Configure Reward Intervals:

```env
GIG_SEARCH_INTERVAL=3     # Check for new tasks every 3 hours
GIG_ACTION_INTERVAL=12    # Process actions every 12 hours
GIG_CLAIM_INTERVAL=24     # Process claims every 24 hours
```

3. Monitor Claims:

```typescript
const response = await fetch('https://gigbot.xyz/api/claim?identifiers=499205,mitev_andon');
const claimableTokens = await response.json();
```

## Community Engagement Tracking

### Scenario

Track engagement across multiple platforms and reward active participants.

### Implementation

1. Create Multiple Gigs:

```typescript
const gigs = [
  {
    chain: "base",
    token_address: "0x4ed4...",
    amount: 5,
    duration: "1week",
    gig_type: "like",
    platform: "farcaster"
  },
  {
    chain: "base",
    token_address: "0x4ed4...",
    amount: 10,
    duration: "1week",
    gig_type: "recast",
    platform: "farcaster"
  }
];

for (const gig of gigs) {
  await createAndFundGig(gig);
}
```

2. Track Engagement:

```typescript
async function checkEngagement(identifiers) {
  const claims = await fetch(`https://gigbot.xyz/api/claim?identifiers=${identifiers}`);
  const data = await claims.json();

  return {
    totalTokens: data.summary.total_tokens,
    totalGigs: data.summary.total_gigs,
    // Add custom metrics
  };
}
```

## Custom Bounty Program

### Scenario

Create a custom bounty program for specific community actions.

### Implementation

1. Create Custom Bounty:

```typescript
const customBounty = {
  chain: "base",
  token_address: "0x4ed4...",
  amount: 100,
  duration: "1month",
  gig_type: "custom_bounty",
  how_to_earn: "Create a tutorial video about our product",
  earning_criteria: "Video must be at least 5 minutes long and cover key features"
};

const response = await createGig(customBounty);
```

2. Verify and Process Claims:

```typescript
// Manual verification might be needed for custom bounties
async function verifyCustomBounty(claimUrl) {
  const response = await fetch('https://gigbot.xyz/api/claim', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ url: claimUrl })
  });

  return response.json();
}
```

## Best Practices

1. **Rate Limiting**

   - Implement exponential backoff for API calls
   - Respect API rate limits
   - Cache responses when possible

2. **Error Handling**

   - Implement proper error handling
   - Log failed transactions
   - Set up monitoring for critical operations

3. **Security**

   - Rotate API keys regularly
   - Monitor wallet balances
   - Implement proper access controls

4. **Monitoring**
   - Track successful/failed claims
   - Monitor engagement metrics
   - Set up alerts for unusual activity
