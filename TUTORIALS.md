# Tutorials

> Step-by-step guides to help you get started with axionax

## 📚 Table of Contents

### Beginner Tutorials
1. [Your First Transaction](#1-your-first-transaction)
2. [Deploy a Simple Contract](#2-deploy-a-simple-contract)
3. [Query Blockchain Data](#3-query-blockchain-data)

### Intermediate Tutorials
4. [Build a Token Contract](#4-build-a-token-contract)
5. [Create an NFT Collection](#5-create-an-nft-collection)
6. [Build a DeFi App](#6-build-a-defi-app)

### Advanced Tutorials
7. [Implement Cross-Chain Bridge](#7-implement-cross-chain-bridge)
8. [Build a DeAI Application](#8-build-a-deai-application)
9. [Optimize Gas Usage](#9-optimize-gas-usage)

---

## 1. Your First Transaction

### Setup

```bash
# Install axionax SDK
npm install @axionax/sdk

# Or use TypeScript
npm install @axionax/sdk @types/node
```

### Create a Wallet

```typescript
import { Wallet } from '@axionax/sdk';

// Generate new wallet
const wallet = Wallet.generate();

console.log('Address:', wallet.address);
console.log('Private Key:', wallet.privateKey);

// Save securely (never share!)
```

### Get Test Tokens

```bash
# Visit testnet faucet
https://faucet.axionax.network

# Enter your address
# Receive 10 AXX for testing
```

### Send Transaction

```typescript
import { AxionaxClient, Wallet } from '@axionax/sdk';

const client = new AxionaxClient({
  rpcUrl: 'https://testnet-rpc.axionax.network',
  chainId: 86137
});

const wallet = Wallet.fromPrivateKey('your-private-key');

async function sendTransaction() {
  const tx = await client.sendTransaction({
    from: wallet.address,
    to: '0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb',
    value: '1000000000000000000', // 1 AXX
    gasLimit: 21000
  });

  console.log('Transaction hash:', tx.hash);
  
  // Wait for confirmation
  const receipt = await tx.wait();
  console.log('Confirmed in block:', receipt.blockNumber);
}

sendTransaction();
```

**Expected Output:**
```
Transaction hash: 0x123...abc
Confirmed in block: 12345
```

---

## 2. Deploy a Simple Contract

### Write Contract

```rust
// contracts/counter.rs
use axionax_sdk::prelude::*;

#[contract]
pub struct Counter {
    value: u64,
}

#[contract_impl]
impl Counter {
    #[init]
    pub fn new() -> Self {
        Self { value: 0 }
    }

    #[public]
    pub fn increment(&mut self) {
        self.value += 1;
    }

    #[view]
    pub fn get(&self) -> u64 {
        self.value
    }
}
```

### Compile

```bash
# Compile to WASM
cargo build --target wasm32-unknown-unknown --release

# Output: target/wasm32-unknown-unknown/release/counter.wasm
```

### Deploy

```typescript
import { ContractFactory } from '@axionax/sdk';
import fs from 'fs';

const wasm = fs.readFileSync('counter.wasm');

async function deploy() {
  const factory = new ContractFactory(wasm, wallet);
  const contract = await factory.deploy();
  
  console.log('Contract deployed at:', contract.address);
  
  return contract;
}
```

### Interact

```typescript
async function interact() {
  const contract = await deploy();
  
  // Call increment
  const tx = await contract.increment();
  await tx.wait();
  
  // Read value
  const value = await contract.get();
  console.log('Counter value:', value); // 1
}
```

---

## 3. Query Blockchain Data

### Query Blocks

```typescript
// Get latest block
const latestBlock = await client.getBlock('latest');
console.log('Block number:', latestBlock.number);
console.log('Timestamp:', latestBlock.timestamp);
console.log('Transactions:', latestBlock.transactions.length);

// Get specific block
const block = await client.getBlock(12345);
```

### Query Transactions

```typescript
// Get transaction by hash
const tx = await client.getTransaction(
  '0x123...abc'
);

console.log('From:', tx.from);
console.log('To:', tx.to);
console.log('Value:', tx.value);
console.log('Gas used:', tx.gasUsed);
```

### Query Account Balance

```typescript
// Get balance
const balance = await client.getBalance(
  '0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb'
);

console.log('Balance:', balance, 'AXX');
```

### Query Events

```typescript
// Query contract events
const events = await contract.queryFilter(
  contract.filters.Transfer(),
  fromBlock,
  toBlock
);

for (const event of events) {
  console.log('Transfer:', {
    from: event.args.from,
    to: event.args.to,
    amount: event.args.amount
  });
}
```

---

## 4. Build a Token Contract

### ERC20-like Token

```rust
use axionax_sdk::prelude::*;

#[contract]
pub struct Token {
    total_supply: u64,
    balances: HashMap<Address, u64>,
    allowances: HashMap<(Address, Address), u64>,
}

#[contract_impl]
impl Token {
    #[init]
    pub fn new(initial_supply: u64) -> Self {
        let mut balances = HashMap::new();
        balances.insert(msg::sender(), initial_supply);
        
        Self {
            total_supply: initial_supply,
            balances,
            allowances: HashMap::new(),
        }
    }

    #[public]
    pub fn transfer(
        &mut self,
        to: Address,
        amount: u64
    ) -> Result<()> {
        let from = msg::sender();
        
        require!(
            self.balances.get(&from).unwrap_or(&0) >= &amount,
            "Insufficient balance"
        );
        
        *self.balances.entry(from).or_insert(0) -= amount;
        *self.balances.entry(to).or_insert(0) += amount;
        
        emit!(Transfer { from, to, amount });
        Ok(())
    }

    #[view]
    pub fn balance_of(&self, account: Address) -> u64 {
        *self.balances.get(&account).unwrap_or(&0)
    }
}
```

### Deploy & Test

```typescript
const Token = await ethers.getContractFactory('Token');
const token = await Token.deploy(1000000);

// Transfer tokens
await token.transfer(recipient, 100);

// Check balance
const balance = await token.balanceOf(recipient);
console.log('Balance:', balance); // 100
```

---

## 5. Create an NFT Collection

### NFT Contract

```rust
#[contract]
pub struct NFT {
    name: String,
    symbol: String,
    owners: HashMap<u64, Address>,
    token_uris: HashMap<u64, String>,
    next_token_id: u64,
}

#[contract_impl]
impl NFT {
    #[init]
    pub fn new(name: String, symbol: String) -> Self {
        Self {
            name,
            symbol,
            owners: HashMap::new(),
            token_uris: HashMap::new(),
            next_token_id: 1,
        }
    }

    #[public]
    pub fn mint(
        &mut self,
        to: Address,
        token_uri: String
    ) -> Result<u64> {
        let token_id = self.next_token_id;
        self.next_token_id += 1;
        
        self.owners.insert(token_id, to);
        self.token_uris.insert(token_id, token_uri);
        
        emit!(Transfer {
            from: Address::zero(),
            to,
            token_id
        });
        
        Ok(token_id)
    }

    #[view]
    pub fn owner_of(&self, token_id: u64) -> Option<Address> {
        self.owners.get(&token_id).copied()
    }
}
```

---

## 6. Build a DeFi App

Coming soon! Topics will include:
- Automated Market Maker (AMM)
- Lending Protocol
- Yield Farming
- Staking

---

## 7. Implement Cross-Chain Bridge

Coming soon! Learn about:
- Bridge Architecture
- Security Considerations
- Asset Locking/Unlocking
- Message Passing

---

## 8. Build a DeAI Application

Coming soon! Topics:
- Decentralized AI Inference
- Model Deployment
- Data Privacy
- Reward Distribution

---

## 9. Optimize Gas Usage

Coming soon! Learn:
- Storage Optimization
- Batch Operations
- Gas Profiling
- Best Practices

---

## 🔗 Resources

- [Developer Guide](./DEVELOPER_GUIDE.md)
- [API Reference](./API_REFERENCE.md)
- [Example Projects](https://github.com/axionaxprotocol/examples)
- [Community Forum](https://forum.axionax.network)

## 💬 Get Help

- [Discord](https://discord.gg/axionax)
- [GitHub Discussions](https://github.com/axionaxprotocol/axionax-core/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/axionax)

---

**Ready to build?** Start with [Tutorial 1](#1-your-first-transaction) or jump to any section above!
