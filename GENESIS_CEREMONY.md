# Genesis Ceremony & Network Launch
## axionax v1.6 Testnet Public Launch

> **เอกสารนี้**: ขั้นตอนการจัด Genesis Ceremony และ Launch เครือข่าย Testnet อย่างเป็นทางการ

## 📋 สารบัญ
- [Overview](#overview)
- [Roles & Responsibilities](#roles--responsibilities)
- [Timeline](#timeline)
- [Genesis Configuration](#genesis-configuration)
- [Validator Registration](#validator-registration)
- [Genesis Ceremony Steps](#genesis-ceremony-steps)
- [Network Launch](#network-launch)
- [Post-Launch Validation](#post-launch-validation)

---

## Overview

**Genesis Ceremony** คือกระบวนการสร้าง **Genesis Block** (บล็อกแรก) ของเครือข่าย axionax Testnet ซึ่งประกอบด้วย:
- รายชื่อ Validators เริ่มต้น (Genesis Validators)
- การจัดสรรโทเค็น (Token Allocations)
- พารามิเตอร์เครือข่าย (Network Parameters)
- สัญญา Smart Contracts เริ่มต้น

**เป้าหมาย**:
- ✅ สร้าง genesis.json ที่ถูกต้องและตรวจสอบได้
- ✅ รับรอง Validators ทั้งหมดมี genesis file เดียวกัน
- ✅ Launch เครือข่ายอย่างปลอดภัยและเสถียร
- ✅ Decentralized และ Trustless

---

## Roles & Responsibilities

### 1. Genesis Coordinator (คุณ/ทีม Core)
- **ความรับผิดชอบ**:
  - จัด Genesis Ceremony
  - รวบรวม validator public keys
  - สร้าง genesis.json
  - แจกจ่าย genesis file ให้ validators
  - ประกาศ genesis hash อย่างเป็นทางการ
  - Bootstrap node แรก

### 2. Genesis Validators (5-10 nodes แนะนำ)
- **ความรับผิดชอบ**:
  - สร้าง validator keypair
  - ส่ง public key + node info ให้ Coordinator
  - ตรวจสอบ genesis.json
  - พร้อม launch ตามเวลาที่กำหนด
  - Maintain node stability หลัง launch

### 3. Community (Optional Observers)
- **ความรับผิดชอบ**:
  - ตรวจสอบความโปร่งใสของกระบวนการ
  - Verify genesis hash
  - รัน full nodes (non-validator)

---

## Timeline

### Phase 1: Preparation (2-3 weeks before launch)
**Week 1-2**:
- [ ] ประกาศ Genesis Launch date
- [ ] เปิดรับสมัคร Genesis Validators
- [ ] แจกจ่ายเอกสาร VPS setup
- [ ] Setup communication channels (Discord/Telegram)

**Week 3**:
- [ ] Validators ส่ง public keys
- [ ] Coordinator รวบรวมข้อมูล validators
- [ ] ตรวจสอบความพร้อมของ validators

### Phase 2: Genesis Creation (1 week before launch)
**Day -7 to -3**:
- [ ] สร้าง genesis.json draft
- [ ] Validators review และ verify
- [ ] Finalize genesis parameters

**Day -2 to -1**:
- [ ] ประกาศ genesis.json อย่างเป็นทางการ
- [ ] แจกจ่าย genesis file + bootstrap nodes
- [ ] Validators initialize nodes
- [ ] Dry run tests

### Phase 3: Launch Day (D-Day)
**Launch Time: 00:00 UTC (กำหนดเวลาที่แน่นอน)**
- [ ] T-15 min: Final validator readiness check
- [ ] T-5 min: All validators start syncing
- [ ] T-0: Genesis Coordinator starts first node
- [ ] T+1 min: Other validators join network
- [ ] T+5 min: Verify consensus reached
- [ ] T+30 min: Open public RPC endpoints

### Phase 4: Post-Launch (First 24 hours)
- [ ] Monitor block production
- [ ] Verify all validators participating
- [ ] Check network health metrics
- [ ] Announce successful launch
- [ ] Enable public access (faucet, explorer)

---

## Genesis Configuration

### Network Parameters
```yaml
# Chain Configuration
chain_id: 86137                    # axionax Testnet
network_name: "axionax Testnet"
chain_name: "axi-testnet"

# Genesis Time
genesis_time: "2025-01-15T00:00:00Z"  # แก้ไขตามวันที่จริง

# Block Parameters
block_time: 5s                     # 5 seconds per block
max_block_size: 5242880           # 5 MB
max_gas_limit: 30000000           # 30M gas

# Consensus
consensus_algorithm: "PoPC"        # Proof-of-Probabilistic-Checking
epoch_length: 100                  # blocks per epoch
min_validator_stake: 10000         # 10,000 AXX minimum
max_validators: 100                # Maximum validator set size
```

### Token Allocations
```yaml
# Total Supply: 1,000,000,000 AXX (1 billion)
allocations:
  # Foundation (30%)
  - address: "0x..."
    balance: "300000000000000000000000000"  # 300M AXX
    vesting: true
    vesting_schedule: "4 years linear"
  
  # Validators Rewards Pool (25%)
  - address: "0x..."
    balance: "250000000000000000000000000"  # 250M AXX
    type: "rewards_pool"
  
  # Community & Ecosystem (20%)
  - address: "0x..."
    balance: "200000000000000000000000000"  # 200M AXX
    vesting: true
  
  # Team & Advisors (15%)
  - address: "0x..."
    balance: "150000000000000000000000000"  # 150M AXX
    vesting: true
    vesting_schedule: "2 years cliff, 4 years linear"
  
  # Public Sale (10%)
  - address: "0x..."
    balance: "100000000000000000000000000"  # 100M AXX
  
  # Genesis Validators (split among validators)
  # Each validator gets initial stake of 50,000 AXX
  - address: "0xValidator1..."
    balance: "50000000000000000000000"     # 50K AXX
  - address: "0xValidator2..."
    balance: "50000000000000000000000"
  # ... (continue for all genesis validators)
```

### Smart Contracts (Pre-deployed)
```yaml
contracts:
  # AXX Token (Native Wrapped Token)
  - name: "AXX_Token"
    address: "0x0000000000000000000000000000000000001000"
    bytecode: "..."
  
  # Staking Contract
  - name: "ValidatorStaking"
    address: "0x0000000000000000000000000000000000001001"
    bytecode: "..."
  
  # PoPC Coordinator
  - name: "POPCCoordinator"
    address: "0x0000000000000000000000000000000000001002"
    bytecode: "..."
```

---

## Validator Registration

### 1. Generate Validator Keys
แต่ละ validator ต้องสร้าง keypair:
```bash
# Run on validator VPS
axionax-core keys generate --output ~/validator-keys.json

# Output example:
{
  "address": "0x1234567890abcdef1234567890abcdef12345678",
  "public_key": "0xabcd1234...",
  "private_key": "KEEP_THIS_SECRET"
}
```

### 2. Submit Information to Coordinator
ส่งข้อมูลต่อไปนี้ให้ Genesis Coordinator (ผ่าน encrypted channel):

```json
{
  "validator_name": "Validator-01",
  "operator": "Your Name / Organization",
  "address": "0x1234567890abcdef1234567890abcdef12345678",
  "public_key": "0xabcd1234...",
  "node_info": {
    "enode": "enode://NODE_ID@PUBLIC_IP:30303",
    "ip": "123.456.789.012",
    "p2p_port": 30303,
    "location": "Singapore",
    "hardware": "8 vCPU, 32GB RAM, 1TB SSD"
  },
  "contact": {
    "email": "validator@example.com",
    "telegram": "@validator_handle",
    "discord": "validator#1234"
  },
  "commitment": {
    "stake_amount": "50000",  # AXX to stake
    "uptime_commitment": "99.9%",
    "maintenance_window": "Sunday 00:00-02:00 UTC"
  }
}
```

**Deadline**: 7 days before genesis launch

### 3. Coordinator Verification
Coordinator จะ:
- ✅ ตรวจสอบ public key format
- ✅ Verify enode connectivity
- ✅ Check stake commitment meets minimum
- ✅ Confirm validator identity

---

## Genesis Ceremony Steps

### Step 1: Create Genesis Template
Coordinator สร้าง genesis template:

```bash
# Use genesis creation tool
cd ~/axionax-core/tools
./create_genesis.sh \
  --chain-id 86137 \
  --genesis-time "2025-01-15T00:00:00Z" \
  --validators validators.json \
  --allocations allocations.json \
  --output genesis-draft.json
```

### Step 2: Add Validators
เพิ่มข้อมูล validators ลงใน genesis:

```json
{
  "config": {
    "chainId": 86137,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "londonBlock": 0
  },
  "difficulty": "0x1",
  "gasLimit": "0x1c9c380",
  "timestamp": "0x63c3e1e0",
  "extraData": "0x",
  "validators": [
    {
      "address": "0xValidator1Address",
      "stake": "50000000000000000000000",
      "commission": "0.10",
      "name": "Validator-01",
      "enode": "enode://..."
    },
    {
      "address": "0xValidator2Address",
      "stake": "50000000000000000000000",
      "commission": "0.10",
      "name": "Validator-02",
      "enode": "enode://..."
    }
  ],
  "alloc": {
    "0xFoundationAddress": {
      "balance": "300000000000000000000000000"
    },
    "0xValidator1Address": {
      "balance": "50000000000000000000000"
    },
    "0xValidator2Address": {
      "balance": "50000000000000000000000"
    }
  }
}
```

### Step 3: Finalize Genesis
```bash
# Validate genesis file
axionax-core genesis validate --file genesis-draft.json

# Calculate genesis hash
sha256sum genesis-draft.json

# Sign genesis (Coordinator)
axionax-core genesis sign --file genesis-draft.json --key coordinator.key

# Output: genesis.json (final)
```

### Step 4: Distribute Genesis
```bash
# Upload to public location
# 1. GitHub Release
gh release create v1.6.0-genesis --notes "axionax Testnet Genesis" genesis.json

# 2. IPFS (immutable)
ipfs add genesis.json
# Output: QmXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX

# 3. Public download server
scp genesis.json user@testnet.axionax.org:/var/www/html/genesis.json
```

**Announce Publicly**:
```
🎉 axionax Testnet Genesis Released!

Chain ID: 86137
Genesis Time: 2025-01-15T00:00:00Z
Genesis Hash: 0xabcd1234567890abcdef1234567890abcdef1234567890abcdef1234567890ab

Download:
- GitHub: https://github.com/axionaxprotocol/axionax-core/releases/download/v1.6.0-genesis/genesis.json
- IPFS: ipfs://QmXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX
- Direct: https://testnet.axionax.org/genesis.json

Genesis Validators: 10
Total Stake: 500,000 AXX
Block Time: 5s

Launch Countdown: https://testnet.axionax.org/countdown
```

### Step 5: Validators Verify
แต่ละ validator ต้อง verify:

```bash
# Download genesis
wget https://testnet.axionax.org/genesis.json -O ~/.axionax/config/genesis.json

# Verify hash
GENESIS_HASH=$(sha256sum ~/.axionax/config/genesis.json | awk '{print $1}')
echo "Genesis Hash: $GENESIS_HASH"

# Must match: 0xabcd1234567890abcdef1234567890abcdef1234567890abcdef1234567890ab

# Check validator is included
cat ~/.axionax/config/genesis.json | jq '.validators[] | select(.address=="YOUR_VALIDATOR_ADDRESS")'

# Initialize node with genesis
axionax-core init --config ~/.axionax/config/config.yaml \
                  --genesis ~/.axionax/config/genesis.json
```

---

## Network Launch

### Pre-Launch Checklist (T-15 minutes)
Coordinator ประสานกับทุก validator:

```
[] All validators ready?
[] Genesis file verified?
[] Nodes initialized?
[] Network connectivity OK?
[] Monitoring setup complete?
[] Emergency contacts confirmed?
```

### Launch Sequence

**T-10 minutes**: Final standby
```bash
# All validators prepare
sudo systemctl enable axionax-validator
# Don't start yet!
```

**T-5 minutes**: Coordinator starts first
```bash
# Coordinator node (Bootstrap node)
sudo systemctl start axionax-validator

# Wait for node to be ready
journalctl -u axionax-validator -f
# Look for: "Waiting for peers..."
```

**T-2 minutes**: Get bootstrap enode
```bash
# Coordinator gets enode URL
curl -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"admin_nodeInfo","params":[],"id":1}' | jq -r '.result.enode'

# Output: enode://abcd1234@123.456.789.012:30303
# Share this with all validators
```

**T-1 minute**: Update bootstrap nodes
```bash
# All validators update config.yaml
nano ~/.axionax/config/config.yaml

# Add coordinator's enode:
network:
  bootstrap_nodes:
    - "enode://abcd1234@123.456.789.012:30303"
```

**T-0 (Genesis Time)**: Validators launch!
```bash
# All validators execute simultaneously
sudo systemctl start axionax-validator

# Monitor connection
journalctl -u axionax-validator -f
```

**T+1 minute**: Verify peering
```bash
# Check peer count
curl -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}'

# Should see peers connecting
# Expected: Gradually increase to 9-10 peers
```

**T+5 minutes**: Verify consensus
```bash
# Check latest block
curl -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'

# Should see block numbers increasing
# Block 1, Block 2, Block 3... (every 5 seconds)
```

**T+10 minutes**: Validator participation
```bash
# Check who is producing blocks
for i in {1..20}; do
  BLOCK=$(curl -s -X POST http://127.0.0.1:8545 \
    -H "Content-Type: application/json" \
    -d "{\"jsonrpc\":\"2.0\",\"method\":\"eth_getBlockByNumber\",\"params\":[\"latest\",false],\"id\":1}" | jq -r '.result.miner')
  echo "Block miner: $BLOCK"
  sleep 5
done

# Verify all validators producing blocks
```

---

## Post-Launch Validation

### Health Checks (First Hour)

**1. Network Consensus**
```bash
# All nodes should be on same block
for validator in validator1 validator2 validator3; do
  ssh $validator "curl -s -X POST http://127.0.0.1:8545 \
    -H 'Content-Type: application/json' \
    -d '{\"jsonrpc\":\"2.0\",\"method\":\"eth_blockNumber\",\"params\":[],\"id\":1}' | jq -r '.result'"
done
# All should return same block number
```

**2. Validator Participation**
```bash
# Check missed blocks
curl -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"axn_getValidatorStats","params":["YOUR_ADDRESS"],"id":1}' | jq '.'

# Look for:
# - blocks_proposed
# - blocks_missed (should be 0)
# - participation_rate (should be ~100%)
```

**3. Network Health**
```bash
# Check network metrics
curl http://localhost:9090/metrics | grep axionax_

# Key metrics:
# - axionax_peer_count (should be ~10)
# - axionax_block_height (increasing)
# - axionax_consensus_participation (near 100%)
```

### Enable Public Access (T+30 minutes)

**1. Open RPC Endpoints**
```bash
# Update nginx config for public RPC
sudo nano /etc/nginx/sites-available/axionax-rpc

server {
    listen 443 ssl http2;
    server_name testnet-rpc.axionax.org;
    
    location / {
        proxy_pass http://127.0.0.1:8545;
        # Add rate limiting
    }
}

# Reload nginx
sudo systemctl reload nginx
```

**2. Start Faucet**
```bash
cd ~/axionax-core/environments/testnet/axionax_v1.6_Testnet_in_a_Box/faucet
docker-compose up -d

# Test faucet
curl 'http://testnet-faucet.axionax.org/request?address=0xTEST_ADDRESS'
```

**3. Launch Block Explorer**
```bash
cd ~/axionax-core/environments/testnet/axionax_v1.6_Testnet_in_a_Box
docker-compose up -d blockscout

# Wait 5 minutes for indexing
# Access: https://testnet-explorer.axionax.org
```

### Public Announcement (T+1 hour)

```markdown
🎊 axionax Testnet is LIVE! 🎊

Network Information:
- Chain ID: 86137
- Network Name: axionax Testnet
- RPC URL: https://testnet-rpc.axionax.org
- Explorer: https://testnet-explorer.axionax.org
- Faucet: https://testnet-faucet.axionax.org

Genesis:
- Block 0 Time: 2025-01-15T00:00:00Z
- Genesis Hash: 0xabcd...
- Initial Validators: 10
- Current Block: 720+ (1 hour of operation)

Network Stats:
- Block Time: ~5s
- Validators Online: 10/10 (100%)
- Avg Participation: 99.8%
- TPS: 150+

Get Started:
- Add Network to MetaMask: https://testnet.axionax.org/add-network
- Request Test Tokens: https://testnet-faucet.axionax.org
- Read Docs: https://docs.axionax.org
- Join Community: https://discord.gg/axionax

Let's build together! 🚀
```

---

## 🚨 Emergency Procedures

### Network Stall (No blocks being produced)
```bash
# 1. Check all validators
for v in validator1 validator2 ...; do
  ssh $v "systemctl status axionax-validator"
done

# 2. Check logs for errors
journalctl -u axionax-validator -n 100 --no-pager | grep -i error

# 3. Coordinate restart (if needed)
# All validators execute:
sudo systemctl restart axionax-validator
```

### Chain Fork Detected
```bash
# 1. Identify which validators on wrong chain
for v in validator1 validator2 ...; do
  BLOCK_HASH=$(ssh $v "curl -s -X POST http://127.0.0.1:8545 \
    -H 'Content-Type: application/json' \
    -d '{\"jsonrpc\":\"2.0\",\"method\":\"eth_getBlockByNumber\",\"params\":[\"latest\",false],\"id\":1}' | jq -r '.result.hash'")
  echo "$v: $BLOCK_HASH"
done

# 2. Stop minority chain validators
ssh minority_validator "sudo systemctl stop axionax-validator"

# 3. Resync from correct chain
ssh minority_validator "rm -rf ~/.axionax/data/blockchain && axionax-core init ..."

# 4. Restart
ssh minority_validator "sudo systemctl start axionax-validator"
```

### Coordinator Node Down
```bash
# Any validator can become bootstrap node
# 1. Get current node's enode
curl -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"admin_nodeInfo","params":[],"id":1}' | jq -r '.result.enode'

# 2. Share with other validators to add as peer
curl -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"admin_addPeer","params":["ENODE_URL"],"id":1}'
```

---

## 📊 Success Criteria

Genesis Launch ถือว่าสำเร็จเมื่อ:
- ✅ All genesis validators online (10/10)
- ✅ Blocks produced consistently (>720 blocks in 1 hour)
- ✅ No chain forks or consensus issues
- ✅ Average block time: 5-6 seconds
- ✅ Validator participation: >95%
- ✅ Public RPC endpoints accessible
- ✅ Block explorer syncing correctly
- ✅ Faucet operational
- ✅ No security incidents
- ✅ Community able to connect and transact

---

## 📞 Support Channels

**During Genesis Ceremony** (Live support):
- Discord: #genesis-launch channel
- Telegram: Validators Group (private)
- Emergency Hotline: +XX-XXX-XXX-XXXX

**After Launch**:
- Validator Support: validators@axionax.org
- Technical Issues: support@axionax.org
- Documentation: https://docs.axionax.org

---

**Next**: After successful launch, see [VALIDATOR_OPERATIONS.md](./VALIDATOR_OPERATIONS.md) for ongoing maintenance
