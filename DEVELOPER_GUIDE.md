# Developer Guide

> Complete guide for developers building on axionax protocol

## 📋 Table of Contents

- [Environment Setup](#environment-setup)
- [Development Workflow](#development-workflow)
- [Building Smart Contracts](#building-smart-contracts)
- [Testing](#testing)
- [API Integration](#api-integration)
- [Best Practices](#best-practices)

## 🛠️ Environment Setup

### Required Tools

```bash
# Check versions
rust --version      # >= 1.70.0
node --version      # >= 18.0.0
python3 --version   # >= 3.10
docker --version    # >= 20.10
```

### Quick Setup

**Linux/macOS:**
```bash
curl -sSL https://raw.githubusercontent.com/axionaxprotocol/axionax-core/main/scripts/install_dependencies_linux.sh | bash
```

**Windows (PowerShell as Admin):**
```powershell
irm https://raw.githubusercontent.com/axionaxprotocol/axionax-core/main/scripts/install_dependencies_windows.ps1 | iex
```

### IDE Configuration

**VS Code Extensions:**
- rust-analyzer
- Even Better TOML
- CodeLLDB
- axionax Protocol Extension (coming soon)

**VS Code Settings:**
```json
{
  "rust-analyzer.cargo.features": "all",
  "rust-analyzer.checkOnSave.command": "clippy",
  "[rust]": {
    "editor.formatOnSave": true
  }
}
```

## 🔄 Development Workflow

### 1. Clone and Setup

```bash
# Clone repository
git clone https://github.com/axionaxprotocol/axionax-core.git
cd axionax-core

# Install dependencies
cargo build --release

# Run setup
./scripts/setup_dev_env.sh
```

### 2. Project Structure

```
axionax-core/
├── core/                 # Core protocol implementation
│   ├── consensus/       # PoPC consensus
│   ├── network/         # P2P networking
│   └── state/           # State management
├── bridge/              # Cross-chain bridge
├── deai/                # Decentralized AI
├── sdk/                 # TypeScript SDK
└── tests/               # Integration tests
```

### 3. Running Local Node

```bash
# Start local node
cargo run --release --bin axionax-node -- \
  --chain-id 31337 \
  --rpc-port 8545 \
  --ws-port 8546

# Check status
curl http://localhost:8545/status
```

## 📝 Building Smart Contracts

### Contract Template

```rust
use axionax_sdk::prelude::*;

#[contract]
pub struct MyContract {
    owner: Address,
    value: u64,
}

#[contract_impl]
impl MyContract {
    #[init]
    pub fn new(owner: Address) -> Self {
        Self { owner, value: 0 }
    }

    #[public]
    pub fn set_value(&mut self, new_value: u64) -> Result<()> {
        require!(msg::sender() == self.owner, "Only owner");
        self.value = new_value;
        Ok(())
    }

    #[view]
    pub fn get_value(&self) -> u64 {
        self.value
    }
}
```

### Compile & Deploy

```bash
# Compile contract
cargo build --target wasm32-unknown-unknown --release

# Deploy to testnet
axionax-cli deploy \
  --contract target/wasm32-unknown-unknown/release/my_contract.wasm \
  --network testnet \
  --gas-limit 1000000
```

## 🧪 Testing

### Unit Tests

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_set_value() {
        let mut contract = MyContract::new(Address::zero());
        contract.set_value(42).unwrap();
        assert_eq!(contract.get_value(), 42);
    }
}
```

### Integration Tests

```bash
# Run all tests
cargo test --workspace

# Run specific test suite
cargo test --package axionax-core --test integration

# Run with logging
RUST_LOG=debug cargo test
```

### Performance Testing

```bash
# Benchmark
cargo bench --package axionax-core

# Load testing
python3 tests/load_test.py --tps 10000 --duration 60
```

## 🔌 API Integration

### REST API

```javascript
// Using TypeScript SDK
import { AxionaxClient } from '@axionax/sdk';

const client = new AxionaxClient({
  rpcUrl: 'https://testnet-rpc.axionax.network',
  chainId: 86137
});

// Query balance
const balance = await client.getBalance('0x...');

// Send transaction
const tx = await client.sendTransaction({
  to: '0x...',
  value: '1000000000000000000', // 1 AXX
  gasLimit: 21000
});
```

### WebSocket API

```javascript
// Subscribe to blocks
const ws = client.ws();

ws.on('newBlock', (block) => {
  console.log('New block:', block.number);
});

ws.on('transaction', (tx) => {
  console.log('New tx:', tx.hash);
});
```

### GraphQL API

```graphql
query {
  block(number: 12345) {
    number
    hash
    timestamp
    transactions {
      hash
      from
      to
      value
    }
  }
}
```

## ✨ Best Practices

### Code Style

```rust
// ✅ Good: Clear function names
pub fn calculate_reward(stake: u64, duration: u64) -> u64 {
    stake * duration / 100
}

// ❌ Bad: Unclear naming
pub fn calc(a: u64, b: u64) -> u64 {
    a * b / 100
}
```

### Error Handling

```rust
// ✅ Good: Use Result types
pub fn transfer(
    from: Address,
    to: Address,
    amount: u64
) -> Result<(), TransferError> {
    if amount == 0 {
        return Err(TransferError::ZeroAmount);
    }
    // ... transfer logic
    Ok(())
}

// ❌ Bad: Panic on errors
pub fn transfer(from: Address, to: Address, amount: u64) {
    assert!(amount > 0, "Amount must be positive");
    // ... transfer logic
}
```

### Security

```rust
// ✅ Good: Check permissions
pub fn withdraw(&mut self, amount: u64) -> Result<()> {
    require!(msg::sender() == self.owner, "Unauthorized");
    require!(self.balance >= amount, "Insufficient balance");
    self.balance -= amount;
    Ok(())
}

// ❌ Bad: No permission checks
pub fn withdraw(&mut self, amount: u64) {
    self.balance -= amount;
}
```

### Gas Optimization

```rust
// ✅ Good: Batch operations
pub fn batch_transfer(
    &mut self,
    recipients: Vec<(Address, u64)>
) -> Result<()> {
    for (to, amount) in recipients {
        self.transfer(to, amount)?;
    }
    Ok(())
}

// ❌ Bad: Individual calls
// (requires N separate transactions)
```

## 📚 Additional Resources

- [API Reference](./API_REFERENCE.md)
- [Architecture Guide](./ARCHITECTURE.md)
- [Testing Guide](./TESTING_GUIDE.md)
- [Security Best Practices](./SECURITY.md)
- [Example Projects](https://github.com/axionaxprotocol/examples)

## 💬 Community & Support

- [Discord](https://discord.gg/axionax)
- [GitHub Discussions](https://github.com/axionaxprotocol/axionax-core/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/axionax)
- [Developer Forum](https://forum.axionax.network)

## 🐛 Reporting Issues

Found a bug? [Open an issue](https://github.com/axionaxprotocol/axionax-core/issues/new) with:
- Clear description
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, versions)

---

**Next Steps:**
- [Build Your First App →](./tutorials/first-app.md)
- [Deploy to Testnet →](./TESTNET_INTEGRATION.md)
- [Explore Examples →](https://github.com/axionaxprotocol/examples)
