# axionax protocol Documentation 📚

Official technical documentation for **axionax protocol** - a Layer-1 blockchain
for high-performance decentralized compute markets.

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Protocol](https://img.shields.io/badge/Protocol-axionax-purple)](https://axionax.org)
[![Jekyll](https://img.shields.io/badge/Jekyll-4.0-red)](https://jekyllrb.com/)
[![Status](https://img.shields.io/badge/Status-Active-green)](https://docs.axionax.org)

---

## � Current Status (Updated November 13, 2025)

### Infrastructure Status: 🟡 7/9 Services Operational (78%)

**VPS Deployment (vmi2895217 - 217.216.109.5)**:

| Service | Port | Status | Details |
|---------|------|--------|---------|
| PostgreSQL | 5432 | ✅ Healthy | Connection pool ready (44h uptime) |
| Redis | 6379 | ✅ Healthy | Cache layer operational (44h uptime) |
| Nginx | 80/443 | ✅ Healthy | SSL configured (44h uptime) |
| RPC Node | 8545/8546 | ✅ Healthy | Mock JSON-RPC server, chainId 888 (18h uptime) |
| Grafana | 3030 | ✅ Healthy | v12.2.1, dashboards accessible (15h uptime) |
| Prometheus | 9090 | ✅ Running | Metrics collection active (15h uptime) |
| Web Interface | 3000 | ✅ Running | axionax-web frontend (23h uptime) |
| Explorer API | 3001 | ❌ Down | Container running but service not responding |
| Faucet API | 3002 | ❌ Down | Container running but service not responding |

**System Resources**:
- VPS: 7.8GB RAM, 4 CPU cores
- RAM: ~12% utilization (optimal)
- CPU: Low load average (excellent)
- Disk: 61GB free space
- Uptime: Multiple days
- Docker: 9 containers (7 operational)
- Network: Docker bridge network healthy

**RPC Implementation**:
- Type: Mock JSON-RPC 2.0 server (Node.js + Express + WebSocket)
- Location: `axionax-deploy/mock-rpc/`
- Features: 15+ Ethereum RPC methods, block auto-increment, tx simulation
- Status: Production interim solution (full axionax-core build pending)

**Monitoring Stack Deployed**:
- ✅ Prometheus running on port 9090
- ✅ Grafana v12.2.1 accessible on port 3030
- ✅ Health check scripts available
- � Explorer/Faucet endpoints need troubleshooting
📚 **Documentation Updates:**
- Updated deployment guides with monitoring stack
- Added health check endpoint documentation
- Fixed Grafana port references (3000 → 3030)
- Added service health monitoring examples

🔥 **Active Work:**
- API reference completion
- Code examples validation
- Integration guide updates
- Tutorial enhancements

---

## 📖 About

This repository contains the **complete technical documentation** for the
axionax protocol, including architecture, API references, guides, and tutorials.

### Part of axionax Ecosystem

Documentation for the entire axionax protocol:

- **Protocol Core**: [`../axionax-core`](../axionax-core) - Blockchain implementation
- **SDK**: [`../axionax-sdk-ts`](../axionax-sdk-ts) - Developer toolkit
- **Web Interface**: [`../axionax-web`](../axionax-web) - Official website
- **Marketplace**: [`../axionax-marketplace`](../axionax-marketplace) - Compute marketplace
- **DevTools**: [`../axionax-devtools`](../axionax-devtools) - Development tools
- **Deploy**: [`../axionax-deploy`](../axionax-deploy) - Infrastructure deployment
- **Issue Manager**: [`../issue-manager`](../issue-manager) - Task tracking

**Main Repository**:
[axionaxprotocol/axionax-core](https://github.com/axionaxprotocol/axionax-core)

**Live Site**: [docs.axionax.org](https://docs.axionax.org)

**Pre-Testnet Status:** Documentation 90%+ complete, active updates ongoing

---

## 📦 Contents

### Core Documentation

- **`ARCHITECTURE.md`** - axionax protocol system design
- **`QUICKSTART.md`** - Quick start guide
- **`GETTING_STARTED.md`** - Detailed setup instructions
- **`API_REFERENCE.md`** - Complete API documentation (📝 Active)
- **`STATE_RPC_USAGE.md`** - RPC usage guide

### Protocol Specifications

- **`NEW_ARCHITECTURE.md`** - v1.6 multi-language architecture
- **`SECURITY.md`** / **`SECURITY_IMPLEMENTATION.md`** - Security model
- **`TOKENOMICS.md`** - Token economics and distribution
- **`GOVERNANCE.md`** - DAO governance mechanism
- **`ROADMAP.md`** - Development roadmap

### Guides & Tutorials

- **`BUILD.md`** - Building from source
- **`TESTING_GUIDE.md`** - Testing strategies (📝 Active)
- **`CONTRIBUTING.md`** - Contribution guidelines
- **`INTEGRATION_MIGRATION_GUIDE.md`** - Integration guide
- **`RPC_NODE_DEPLOYMENT.md`** - Node deployment (📝 Active)
- **`VPS_VALIDATOR_SETUP.md`** - Validator setup

### Operations & Monitoring

- **`MONITORING.md`** - Prometheus/Grafana setup (✨ Updated Nov 12)
- **`HEALTH_CHECKS.md`** - Service health monitoring (✨ New!)
- **`VPS_OPERATIONS.md`** - Production VPS management (✨ Updated)

### Project Status & Launch Info

- **`PROJECT_COMPLETION.md`** - v1.6 completion status
- **`STATUS.md`** - Current development status
- **`TESTNET_LAUNCH.md`** - Testnet launch information (🔥 New!)
- **`TESTNET_LAUNCH_CHECKLIST.md`** - Pre-launch checklist (🔥 New!)

### Community & Support

- **`FAQ.md`** - Frequently asked questions (📝 Active)
- **`TROUBLESHOOTING.md`** - Common issues & solutions (📝 Active)
- **`NETWORK_INFO.md`** - Chain ID, RPC endpoints, contract addresses (🔥 New!)

### Jekyll Theme

- **`index.html`** / **`index.md`** - Landing page
- **`_includes/`** - Theme components
- **`assets/`** - Static assets (CSS, JS, images)
- **`_config.yml`** - Jekyll configuration
- **`CNAME`** - Custom domain (docs.axionax.org)

---

## 🎯 Pre-Testnet Documentation Checklist

Track completion status:

- [x] ✅ Architecture & System Design
- [x] ✅ Quick Start & Getting Started
- [x] ✅ Security Documentation
- [x] ✅ Tokenomics & Governance
- [ ] 📝 API Reference (90% - examples in progress)
- [ ] 📝 RPC Usage Guide (updating endpoints)
- [ ] 📝 Testing Guide (finalizing)
- [ ] 📝 FAQ & Troubleshooting (active)
- [ ] 🔥 Network Information (new!)
- [ ] 🔥 Testnet Launch Checklist (new!)

Use [`../issue-manager`](../issue-manager) to track documentation tasks.

---

## 🚀 Local Development

### Prerequisites

- Ruby 2.7+ with Bundler
- Jekyll 4.0+
- Git

### Setup and Run

```bash
# Install dependencies
cd docs
bundle install

# Serve locally
bundle exec jekyll serve

# Or with live reload
bundle exec jekyll serve --livereload

# Open browser
# http://localhost:4000
```

### Build Static Site

```bash
bundle exec jekyll build
# Output in _site/
```

---

## 📝 Contributing to Documentation

### Adding New Documentation

1. Create Markdown file in `docs/` directory
2. Add front matter (Jekyll metadata)
3. Write documentation content
4. Update navigation if needed
5. Test locally with Jekyll
6. Submit Pull Request

### Documentation Guidelines

- ✅ Use clear, concise language
- ✅ Include code examples with expected outputs
- ✅ Add diagrams where helpful (Mermaid supported)
- ✅ Link to related docs (internal navigation)
- ✅ Keep axionax protocol focus
- ✅ Update table of contents
- ✅ Test all code examples
- ✅ Validate external links (use `check-links.sh` from devtools)

### Example Front Matter

```yaml
---
layout: default
title: Your Documentation Title
description: Brief description
---
```

---

## 🌐 Deployment

### GitHub Pages

This documentation is automatically deployed to GitHub Pages:

1. Push changes to `main` branch
2. GitHub Actions builds Jekyll site
3. Deploys to `gh-pages` branch
4. Available at https://docs.axionax.org

### Custom Domain

Configured in `CNAME` file:

```
docs.axionax.org
```

DNS setup:

```
CNAME docs -> axionaxprotocol.github.io
```

---

## 🔗 axionax protocol Ecosystem

| Component           | Description               | Location                                         | Status     |
| ------------------- | ------------------------- | ------------------------------------------------ | ---------- |
| **Docs** (this)     | Protocol documentation    | `axionax-docs/`                                  | 📝 Active  |
| **Core**            | Blockchain implementation | [`../axionax-core`](../axionax-core)             | ✅ Ready   |
| **Web**             | Web interface             | [`../axionax-web`](../axionax-web)               | ✅ Ready   |
| **SDK**             | TypeScript SDK            | [`../axionax-sdk-ts`](../axionax-sdk-ts)         | ✅ Ready   |
| **Marketplace**     | Compute marketplace       | [`../axionax-marketplace`](../axionax-marketplace) | 🚧 Beta  |
| **DevTools**        | Development tools         | [`../axionax-devtools`](../axionax-devtools)     | ✅ Ready   |
| **Deploy**          | Infrastructure            | [`../axionax-deploy`](../axionax-deploy)         | 🔥 Testing |
| **Issue Manager**   | Task tracking             | [`../issue-manager`](../issue-manager)           | 🎉 New!    |

---

## 📜 License

MIT License - see [LICENSE](LICENSE) file for details.

**Note**: The axionax protocol Core uses AGPLv3. See
[`../axionax-core/LICENSE`](../axionax-core/LICENSE).

---

## 🔗 Links

- **Main Repository**: https://github.com/axionaxprotocol/axionax-core
- **Live Documentation**: https://docs.axionax.org
- **Protocol Website**: https://axionax.org
- **Issues**: https://github.com/axionaxprotocol/axionax-core/issues

---

## 📊 Documentation Coverage

### By Category

- **Core Protocol**: 100% ✅
- **API Reference**: 90% 📝
- **User Guides**: 95% ✅
- **Developer Guides**: 95% ✅
- **Deployment**: 85% 📝
- **Troubleshooting**: 80% 📝

### Recent Additions (November 2025)

- 🔥 Testnet Launch Checklist
- 🔥 Network Information Page
- 📝 Enhanced API Examples
- 📝 FAQ Section Expansion
- 📝 Troubleshooting Guides

---

**Part of the axionax protocol Ecosystem**

**Last Updated**: November 7, 2025
