# Chain ID Configuration

## Overview

axionax Protocol uses different Chain IDs for different network environments to ensure proper network segregation and prevent transaction replay attacks across networks.

## Supported Networks

### Production Networks

| Network | Chain ID | Status | RPC Endpoint | Purpose |
|---------|----------|--------|--------------|---------|
| **Mainnet** | 86150 | Reserved | TBD | Production network for real-world usage |
| **Testnet** | 86137 | Active | Coming Q1 2026 | Public testnet for testing and development |

### Development Networks

| Network | Chain ID | Status | RPC Endpoint | Purpose |
|---------|----------|--------|--------------|---------|
| **Local Development** | 31337 | Active | localhost:8545 | Local development and testing |

---

## Chain ID 31337 (Local Development)

### Purpose
Chain ID `31337` is the standard Ethereum development chain ID used by local blockchain development tools like Hardhat, Anvil, and Ganache. axionax uses this chain ID for local development to maintain compatibility with existing Ethereum tooling.

### When to Use
- Local development on your machine
- Running unit tests
- Integration testing before deploying to testnet
- Rapid prototyping and iteration

### Configuration

#### JavaScript/TypeScript (SDK)
```typescript
import { axionaxClient } from '@axionax/sdk';

const client = new axionaxClient({
  network: 'local',
  chainId: 31337,
  endpoint: 'http://localhost:8545'
});
```

#### Python (DeAI Integration)
```python
from axionax import Axionax

axionax = Axionax(
    chain_id=31337,
    rpc_url="http://localhost:8545"
)
```

#### Docker Compose
```yaml
environment:
  - CHAIN_ID=31337
  - RPC_URL=http://localhost:8545
  - NETWORK=local
```

### Starting Local Node

#### Using axionax-core
```bash
cd axionax-core
cargo run --release -- --dev --chain-id 31337
```

#### Using Docker
```bash
docker-compose -f docker-compose.dev.yml up -d
```

### Security Considerations

⚠️ **Important Security Notes:**

1. **Never use Chain ID 31337 in production** - This chain ID is for development only
2. **Do not send real assets** - All transactions on Chain ID 31337 are for testing
3. **Reset regularly** - Development chains should be reset frequently to maintain clean state
4. **Private keys** - Development private keys should never be used on mainnet or testnet

---

## Chain ID 86137 (Testnet)

### Purpose
Public testnet for community testing, validator onboarding, and dApp development before mainnet launch.

### Status
**Coming Q1 2026** - Currently in preparation phase

### Planned Features
- Faucet for free testnet tokens
- Block explorer
- RPC endpoints in multiple regions
- Smart contract deployment
- Validator participation

### Configuration (Preview)
```typescript
const client = new axionaxClient({
  network: 'testnet',
  chainId: 86137,
  endpoint: 'https://rpc.testnet.axionax.org'
});
```

---

## Chain ID 86150 (Mainnet)

### Purpose
Production mainnet for real-world usage with real economic value.

### Status
**Reserved** - Launch planned for Q2 2026 after successful testnet validation

### Requirements Before Mainnet
- [ ] Security audit completion
- [ ] 3+ months of testnet operation
- [ ] Validator network establishment (minimum 20 validators)
- [ ] Bug bounty program completion
- [ ] Performance validation (45K+ TPS, <0.5s finality)

---

## Switching Between Networks

### Environment Variables
```bash
# Local Development
export AXIONAX_CHAIN_ID=31337
export AXIONAX_RPC_URL=http://localhost:8545

# Testnet
export AXIONAX_CHAIN_ID=86137
export AXIONAX_RPC_URL=https://rpc.testnet.axionax.org

# Mainnet (when available)
export AXIONAX_CHAIN_ID=86150
export AXIONAX_RPC_URL=https://rpc.axionax.org
```

### Configuration File (.axionax.config.json)
```json
{
  "networks": {
    "local": {
      "chainId": 31337,
      "rpcUrl": "http://localhost:8545",
      "accounts": ["0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80"]
    },
    "testnet": {
      "chainId": 86137,
      "rpcUrl": "https://rpc.testnet.axionax.org"
    },
    "mainnet": {
      "chainId": 86150,
      "rpcUrl": "https://rpc.axionax.org"
    }
  },
  "defaultNetwork": "local"
}
```

---

## MetaMask Configuration

### Adding axionax Networks to MetaMask

#### Local Development (31337)
```
Network Name: axionax Local
RPC URL: http://localhost:8545
Chain ID: 31337
Currency Symbol: AXX
Block Explorer: http://localhost:3001 (if running local explorer)
```

#### Testnet (86137) - Coming Soon
```
Network Name: axionax Testnet
RPC URL: https://rpc.testnet.axionax.org
Chain ID: 86137
Currency Symbol: AXX
Block Explorer: https://explorer.testnet.axionax.org
```

#### Mainnet (86150) - Coming Q2 2026
```
Network Name: axionax
RPC URL: https://rpc.axionax.org
Chain ID: 86150
Currency Symbol: AXX
Block Explorer: https://explorer.axionax.org
```

---

## Troubleshooting

### Wrong Chain ID Error
```
Error: Chain ID mismatch. Expected 86137, got 31337
```

**Solution:**
1. Check your RPC URL matches the intended network
2. Verify environment variables are set correctly
3. Clear MetaMask's cached nonce (Settings → Advanced → Reset Account)

### Cannot Connect to Local Node
```
Error: Cannot connect to http://localhost:8545
```

**Solution:**
1. Ensure local node is running: `docker-compose ps`
2. Check firewall settings allow localhost connections
3. Verify port 8545 is not in use by another service

### Transaction on Wrong Network
```
Error: Transaction sent to wrong network
```

**Solution:**
1. Always verify chain ID before signing transactions
2. Use network-specific wallets for different environments
3. Never reuse private keys across networks

---

## Best Practices

### Development Workflow
1. **Start with local (31337)** for rapid iteration
2. **Test on testnet (86137)** before mainnet deployment
3. **Validate thoroughly** on testnet with real scenarios
4. **Deploy to mainnet (86150)** only after complete validation

### Security
- ✅ Use different accounts for each network
- ✅ Never use development keys in production
- ✅ Always verify chain ID before transactions
- ✅ Keep private keys secure and encrypted
- ✅ Use hardware wallets for mainnet

### Testing Strategy
```typescript
// Always specify chain ID explicitly in tests
const CHAIN_IDS = {
  LOCAL: 31337,
  TESTNET: 86137,
  MAINNET: 86150
};

describe('Smart Contract Tests', () => {
  beforeEach(async () => {
    // Ensure we're on local network
    const chainId = await provider.getChainId();
    expect(chainId).to.equal(CHAIN_IDS.LOCAL);
  });
});
```

---

## References

- **EIP-155:** Chain ID specification
- **axionax Architecture:** See [ARCHITECTURE.md](../axionax-docs/ARCHITECTURE.md)
- **Network Status:** Check [https://status.axionax.org](https://status.axionax.org) (coming soon)

---

**Last Updated:** November 11, 2025  
**Version:** 1.0  
**Status:** Active (Local Development) | Planned (Testnet & Mainnet)
