# ElizaOS Plugin Integration Guide

## Overview

The GigBot ElizaOS plugin enables automated task management and reward distribution through your ElizaOS environment. This guide will help you set up and use the plugin effectively.

## Prerequisites

- Node.js and pnpm installed
- ElizaOS runtime
- GigBot API key

## Installation

```bash
pnpm add @elizaos/plugin-gigbot
```

## Configuration

1. Create or edit `.env` file in your project root:

```env
# GigBot API Credentials
GIGBOT_API_URL=https://www.gigbot.xyz/api  # Default API URL for GigBot

# Client Configuration
GIG_SEARCH_INTERVAL=3     # Interval for searching tasks (hours)
GIG_ACTION_INTERVAL=12    # Interval for performing actions (hours)
GIG_CLAIM_INTERVAL=24     # Interval for claiming tasks (hours)
GIG_CLAIM_PLATFORM=x      # Platform for claiming tasks ('x' or 'farcaster')
EVM_PRIVATE_KEY=0x...     # Private key for claiming rewards (must start with 0x)
```

## Plugin Registration

```typescript
import { GigBotClientInterface } from "@elizaos/gigbot";

const gigbotPlugin = {
    name: "gigbot",
    description: "GigBot client",
    clients: [GigBotClientInterface],
};

// Register with your Eliza runtime
runtime.registerPlugin(gigbotPlugin);
```

## Important: Twitter Plugin Dependency

The GigBot plugin requires the Twitter client to be initialized first:

```typescript
export async function initializeClients(
    character: Character,
    runtime: IAgentRuntime
) {
    const clients: ClientInstance[] = [];

    if (character.plugins?.length > 0) {
        for (const plugin of character.plugins) {
            // Check if current plugin is GigBot
            let isGigbot = plugin.name === "@elizaos-plugins/plugin-gigbot";

            if (plugin.clients) {
                for (const client of plugin.clients) {
                    // Pass existing clients to GigBot runtime
                    const startedClient = await client.start(
                        isGigbot ? {...runtime, clients} : runtime
                    );
                    clients.push(startedClient);
                }
            }
        }
    }

    return clients;
}
```

## Features

### Task Automation

- Automatic task discovery
- Task completion tracking
- Reward claiming

### Interaction Handling

- Social media post monitoring
- Engagement tracking
- Reward distribution

## Security Best Practices

1. API Key Security:

   - Store in environment variables
   - Never commit to version control
   - Rotate regularly

2. Wallet Security:
   - Use a dedicated wallet
   - Limit funds
   - Monitor transactions

## Troubleshooting

### Common Issues

1. Plugin Initialization Fails:

   - Verify Twitter plugin is initialized first
   - Check environment variables
   - Confirm API key is valid

2. Task Automation Issues:
   - Check intervals in configuration
   - Verify wallet has sufficient funds
   - Ensure social media accounts are properly linked

### Debug Mode

Enable debug logging:

```bash
DEBUG=eliza:* pnpm start
```

## Support

For issues or questions:

1. Check the troubleshooting guide
2. Review debug logs
3. Open an issue with:
   - Error messages
   - Configuration details
   - Steps to reproduce
