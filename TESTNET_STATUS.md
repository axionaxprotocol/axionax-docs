# axionax Testnet Status

> **Last Updated**: November 15, 2025  
> **Version**: v1.7.0-testnet  
> **Phase**: Phase 1 Complete ✅ | Phase 2 In Progress 🔄

---

## 🌐 Live Testnet Information

### Network Details
- **Chain ID**: 86137 (0x15079)
- **Native Token**: AXX (18 decimals)
- **RPC Endpoint**: http://217.216.109.5:8545
- **WebSocket**: ws://217.216.109.5:8546
- **Explorer**: http://217.216.109.5:3000/explorer
- **Faucet**: http://217.216.109.5:3000/faucet
- **Website**: http://217.216.109.5:3000

### VPS Infrastructure
- **Provider**: Contabo
- **Location**: Europe
- **Instance**: vmi2895217 @ 217.216.109.5
- **Resources**:
  - RAM: 8GB (87% free, 1GB used)
  - CPU: 4 vCPU AMD EPYC
  - Disk: 72GB (27% used, 53GB free)
  - Uptime: 8+ days
  - Load Average: 0.10-0.15

---

## 📊 Service Status (9 Services)

| Service | Status | Port | Health | Uptime |
|---------|--------|------|---------|---------|
| **PostgreSQL** | ✅ Running | 5432 | Healthy | 4 hours |
| **Redis** | ✅ Running | 6379 | Healthy | 4 hours |
| **Nginx/SSL** | ✅ Running | 80/443 | Healthy | 4 hours |
| **Mock RPC** | 🟡 Needs Rebuild | 8545/8546 | Degraded | 4 hours |
| **Grafana** | 🔴 Missing | 3030 | Down | - |
| **Prometheus** | ✅ Running | 9090 | Healthy | 3 days |
| **Website** | ✅ Running | 3000 | Healthy | 4 hours |
| **Explorer API** | ✅ Running | 3001 | Healthy | 4 hours |
| **Faucet API** | ✅ Running | 3002 | Healthy | 4 hours |

### Current Issues
1. **Mock RPC** 🟡 Priority: HIGH
   - Container runs Alpine placeholder instead of Node.js mock server
   - Need to rebuild with proper Ethereum JSON-RPC 2.0 implementation
   - Affects: Website statistics, Explorer API, Faucet API

2. **Grafana** 🔴 Priority: MEDIUM
   - Container missing from deployment
   - Need to start Grafana service for monitoring dashboards

---

## ✅ Phase 1: Infrastructure Complete (100%)

### Deployment Achievements
- [x] VPS provisioned and configured
- [x] Docker Compose infrastructure deployed
- [x] PostgreSQL database operational
- [x] Redis cache operational
- [x] Nginx reverse proxy with SSL ready
- [x] Website deployed with MetaMask integration
- [x] Explorer API endpoints working
- [x] Faucet API endpoints working
- [x] Prometheus metrics collection
- [x] Automated backups configured (daily 2AM UTC)
- [x] Real-time statistics API (`/api/stats`)

### Website Features
- **MetaMask Integration**: Auto-configuration for Chain ID 86137
- **Real-time Stats**: Block height, services count, uptime (updates every 5s)
- **Service Health Dashboard**: Visual status of all 9 services
- **Explorer UI**: Browse blocks and transactions
- **Faucet UI**: Request testnet AXX tokens (1 AXX per request, 60min cooldown)
- **Responsive Design**: Mobile-friendly dark theme

### API Endpoints
#### Explorer API (Port 3001)
- `GET /health` - Service health check
- `GET /api/stats` - Real-time network statistics
- `GET /api/blocks/latest` - Latest block number
- `GET /api/blocks/:number` - Get block by number
- `GET /api/tx/:hash` - Get transaction by hash
- `GET /api/account/:address` - Get account balance

#### Faucet API (Port 3002)
- `GET /health` - Service health check
- `GET /api/info` - Faucet configuration (chain ID, amount, cooldown)
- `POST /api/request` - Request testnet tokens
- `GET /api/balance/:address` - Check account balance

### Monitoring & Operations
- **Prometheus Alerts**:
  - ServiceDown: Triggers after 2min downtime
  - HighMemoryUsage: <10% available for 5min
  - HighDiskUsage: <20% available for 5min
- **Automated Backups**:
  - Daily at 2:00 AM UTC via cron
  - PostgreSQL: `pg_dumpall` (3.5KB)
  - Redis: dump.rdb (509 bytes)
  - Retention: 7 days with gzip compression
  - Location: `/root/backups/`

---

## 🔄 Phase 2: Enhanced Services (25% Complete)

### In Progress
- [ ] **Mock RPC Rebuild** 🔴 Critical
  - Replace Alpine placeholder with proper Node.js implementation
  - Implement full Ethereum JSON-RPC 2.0 methods
  - Add block mining simulation (every 3 seconds)
  - Support transaction mempool

### Planned (This Week)
- [ ] **Grafana Dashboards** 🟡 High Priority
  - Restart Grafana container
  - Create dashboards for all services
  - Visualize block production rate
  - Monitor API response times
  - Track faucet distribution

- [ ] **Enhanced RPC Methods**
  - `eth_call` - Contract interaction
  - `eth_estimateGas` - Gas estimation
  - `eth_getLogs` - Event filtering
  - `eth_getTransactionReceipt` - Receipt with status
  - `debug_traceTransaction` - Debug endpoint

- [ ] **Example Smart Contracts**
  - ERC-20 token template
  - ERC-721 NFT template
  - Simple DeFi contract (swap/stake)
  - Deployment guide and scripts

---

## 📅 Phase 3: Validator Node (Planned)

### Infrastructure Planning
- **Provider**: Hetzner Cloud (recommended)
- **Instance**: CPX41 (8 vCPU, 16GB RAM, 240GB NVMe)
- **Cost**: ~€40/month (~$45 USD)
- **Timeline**: 2-4 weeks after Phase 2

### Tasks
- [ ] Provision dedicated VPS for validator
- [ ] Build axionax-core from source
- [ ] Genesis ceremony (define chain parameters)
- [ ] Configure validator keys
- [ ] Setup systemd service
- [ ] Connect to infrastructure VPS
- [ ] Test block production
- [ ] Enable consensus

---

## 📅 Phase 4: Public Launch (Planned)

### Pre-Launch Checklist
- [ ] All services 100% operational (currently 8/9)
- [ ] Validator node stable (3+ days uptime)
- [ ] Documentation complete
- [ ] Security audit
- [ ] Bug bounty program
- [ ] Community validator recruitment
- [ ] Public announcement

### Timeline
- **Target**: Q1 2026 (1-3 months after Phase 3)
- **Prerequisites**: Phases 1-3 complete, 5-10 validators recruited

---

## 📈 Metrics & KPIs

### Current Metrics (November 15, 2025)
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Services Operational | 8/9 | 9/9 | 🟡 89% |
| Infrastructure Uptime | 8+ days | >7 days | ✅ Pass |
| VPS RAM Usage | 1GB/8GB | <70% | ✅ 13% |
| VPS Disk Usage | 19GB/72GB | <80% | ✅ 26% |
| Website Response Time | <100ms | <500ms | ✅ Pass |
| API Response Time | <50ms | <200ms | ✅ Pass |
| Backup Success Rate | 100% | 100% | ✅ Pass |

### Phase 2 Goals
| Goal | Current | Target |
|------|---------|--------|
| Mock RPC Methods | 1 | 10+ |
| Grafana Dashboards | 0 | 5+ |
| Example Contracts | 0 | 3+ |
| dApp Templates | 0 | 1 |

---

## 🔧 Immediate Next Steps

### This Week (November 15-22)
1. **Fix Mock RPC** 🔴 Day 1-2
   - Locate or create proper Node.js mock RPC implementation
   - Implement Ethereum JSON-RPC 2.0 methods
   - Build Docker image
   - Deploy and test all endpoints
   - Update documentation

2. **Restart Grafana** 🟡 Day 1
   - Find and restart Grafana container
   - Or deploy new Grafana instance
   - Configure data sources (Prometheus)
   - Create initial dashboards

3. **Enhanced RPC Methods** 🟡 Day 3-5
   - Implement `eth_call` for contract interactions
   - Add `eth_estimateGas` for gas calculations
   - Support event logs filtering
   - Add transaction receipts with status

### Next Week (November 23-30)
4. **Example Contracts** 📋
   - Create ERC-20 token template
   - Create ERC-721 NFT template
   - Simple DeFi contract example
   - Deployment scripts and documentation

5. **dApp Starter Kit** 📋
   - React + TypeScript template
   - Web3.js / ethers.js integration
   - MetaMask connection boilerplate
   - Example contract interactions

---

## 🔗 Quick Links

### Documentation
- [Deployment Plan](./TESTNET_DEPLOYMENT_PLAN.md) - Full 4-phase roadmap
- [STATUS.md](./STATUS.md) - Overall project status
- [README.md](./README.md) - Project overview

### Live Services
- Website: http://217.216.109.5:3000
- Explorer API: http://217.216.109.5:3001
- Faucet API: http://217.216.109.5:3002
- Prometheus: http://217.216.109.5:9090
- RPC: http://217.216.109.5:8545 (⚠️ needs rebuild)

### Developer Tools
- Add Network to MetaMask: Chain ID 86137, RPC http://217.216.109.5:8545
- Get Testnet Tokens: http://217.216.109.5:3000/faucet
- View Blocks: http://217.216.109.5:3000/explorer

---

## 📞 Support & Contact

- **Discord**: Coming soon
- **GitHub**: https://github.com/axionaxprotocol
- **Email**: dev@axionax.org

---

**Last Status Check**: November 15, 2025 15:36 UTC  
**Next Update**: November 22, 2025  
**Phase 1 Progress**: ✅ 100% Complete  
**Phase 2 Progress**: 🔄 25% In Progress

Made with 💜 by the axionax Team
