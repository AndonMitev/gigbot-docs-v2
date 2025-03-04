# ElizaOS Plugin

The [GigBot ElizaOS Plugin](https://github.com/PaymagicXYZ/plugin-gigbot) provides seamless integration between GigBot and ElizaOS, enabling automated task management and reward distribution for AI agents.

## Features

- **Task Automation**
  - Automatic task discovery and execution
  - Smart task prioritization
  - Configurable task intervals
- **Reward Management**
  - Automated reward claiming
  - Multi-platform support (X, Farcaster)
  - Token earning optimization
- **Integration Handling**
  - Task collection and tracking
  - Completion verification
  - Discord approval workflow (optional)

## Setup Guide

### Prerequisites

Before you begin, ensure you have:

- Node.js installed
- pnpm package manager
- ElizaOS runtime environment
- Access to GigBot API

### Installation

```bash
pnpm add @elizaos/plugin-gigbot
```

### Configuration

1. Create or modify your `.env` file:

```bash
# GigBot API Configuration
GIGBOT_API_URL=https://www.gigbot.xyz/api

# Task Intervals (in hours)
GIG_SEARCH_INTERVAL=3     # Task discovery frequency
GIG_ACTION_INTERVAL=12    # Task execution frequency
GIG_CLAIM_INTERVAL=24     # Reward claim frequency

# Platform Settings
GIG_CLAIM_PLATFORM=x      # 'x' or 'farcaster'

# Wallet Configuration
EVM_PRIVATE_KEY=0x...     # Your Ethereum private key
```

### Environment Variables

| Variable              | Description                      | Default                    | Required |
| --------------------- | -------------------------------- | -------------------------- | -------- |
| `GIGBOT_API_URL`      | GigBot API endpoint              | https://www.gigbot.xyz/api | No       |
| `GIG_SEARCH_INTERVAL` | Task search frequency (hours)    | 3                          | No       |
| `GIG_ACTION_INTERVAL` | Task execution frequency (hours) | 12                         | No       |
| `GIG_CLAIM_INTERVAL`  | Reward claim frequency (hours)   | 24                         | No       |
| `GIG_CLAIM_PLATFORM`  | Target platform                  | x                          | No       |
| `EVM_PRIVATE_KEY`     | Ethereum wallet private key      | -                          | Yes      |

## Usage Examples

### Basic Plugin Registration

```typescript
import { GigBotClientInterface } from "@elizaos/gigbot";

const gigbotPlugin = {
    name: "gigbot",
    description: "GigBot client",
    clients: [GigBotClientInterface],
};

runtime.registerPlugin(gigbotPlugin);
```

### Advanced Client Initialization

```typescript
export async function initializeClients(
    character: Character,
    runtime: IAgentRuntime
) {
    const clients: ClientInstance[] = [];

    if (character.plugins?.length > 0) {
        for (const plugin of character.plugins) {
            let isGigbot = plugin.name === "@elizaos-plugins/plugin-gigbot";

            if (plugin.clients) {
                for (const client of plugin.clients) {
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

## Security Best Practices

1. **Private Key Management**

   - Never commit private keys to version control
   - Use environment variables for sensitive data
   - Use a dedicated wallet with limited funds

2. **Rate Limiting**

   - Implement proper API rate limiting
   - Monitor API usage and costs
   - Set appropriate task intervals

3. **Error Handling**
   - Implement proper error handling
   - Log failed transactions
   - Monitor task execution status

## Troubleshooting

### Common Issues

1. **Plugin Initialization Fails**

   - Verify Twitter plugin is initialized first
   - Check environment variables
   - Validate API configuration

2. **Task Execution Issues**
   - Check task intervals configuration
   - Verify wallet balance
   - Confirm platform settings

### Debug Mode

Enable debug logging for detailed information:

```bash
DEBUG=eliza:* pnpm start
```

### Testing

Run the test suite:

```bash
pnpm test
```

## Support

If you encounter issues:

1. Check the troubleshooting guide above
2. Enable and review debug logs
3. Open an issue on [GitHub](https://github.com/PaymagicXYZ/plugin-gigbot) with:
   - Error messages
   - Configuration details
   - Steps to reproduce
   - Environment information
