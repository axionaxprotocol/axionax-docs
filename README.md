# AxionAX Protocol Documentation 📚

Official technical documentation for **AxionAX Protocol** - a Layer-1 blockchain
for high-performance decentralized compute markets.

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Protocol](https://img.shields.io/badge/Protocol-AxionAX-purple)](https://axionax.org)
[![Jekyll](https://img.shields.io/badge/Jekyll-4.0-red)](https://jekyllrb.com/)

---

## 📖 About

This repository contains the **complete technical documentation** for the
AxionAX Protocol, including architecture, API references, guides, and tutorials.

### Part of AxionAX Ecosystem

Documentation for the entire AxionAX Protocol:

- **Protocol Core**: [`../core`](../core) - Blockchain implementation
- **SDK**: [`../sdk`](../sdk) - Developer toolkit
- **Web Interface**: [`../web`](../web) - Official website
- **Marketplace**: [`../marketplace`](../marketplace) - Compute marketplace
- **DevTools**: [`../devtools`](../devtools) - Development tools
- **Deploy**: [`../deploy`](../deploy) - Infrastructure deployment

**Main Repository**:
[axionaxprotocol/axionaxiues](https://github.com/axionaxprotocol/axionaxiues)

**Live Site**: [docs.axionax.org](https://docs.axionax.org)

---

## 📦 Contents

### Core Documentation

- **`ARCHITECTURE.md`** - AxionAX Protocol system design
- **`QUICKSTART.md`** - Quick start guide
- **`GETTING_STARTED.md`** - Detailed setup instructions
- **`API_REFERENCE.md`** - Complete API documentation
- **`STATE_RPC_USAGE.md`** - RPC usage guide

### Protocol Specifications

- **`NEW_ARCHITECTURE.md`** - v1.6 multi-language architecture
- **`SECURITY.md`** / **`SECURITY_IMPLEMENTATION.md`** - Security model
- **`TOKENOMICS.md`** - Token economics and distribution
- **`GOVERNANCE.md`** - DAO governance mechanism
- **`ROADMAP.md`** - Development roadmap

### Guides & Tutorials

- **`BUILD.md`** - Building from source
- **`TESTING_GUIDE.md`** - Testing strategies
- **`CONTRIBUTING.md`** - Contribution guidelines
- **`INTEGRATION_MIGRATION_GUIDE.md`** - Integration guide
- **`RPC_NODE_DEPLOYMENT.md`** - Node deployment
- **`VPS_VALIDATOR_SETUP.md`** - Validator setup

### Project Status

- **`PROJECT_COMPLETION.md`** - v1.6 completion status
- **`STATUS.md`** - Current development status
- **`TESTNET_LAUNCH.md`** - Testnet launch information

### Jekyll Theme

- **`index.html`** / **`index.md`** - Landing page
- **`_includes/`** - Theme components
- **`assets/`** - Static assets (CSS, JS, images)
- **`_config.yml`** - Jekyll configuration
- **`CNAME`** - Custom domain (docs.axionax.org)

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
- ✅ Include code examples
- ✅ Add diagrams where helpful
- ✅ Link to related docs
- ✅ Keep AxionAX Protocol focus
- ✅ Update table of contents

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

## 🔗 AxionAX Protocol Ecosystem

| Component       | Description               | Location                           |
| --------------- | ------------------------- | ---------------------------------- |
| **Docs** (this) | Protocol documentation    | `docs/`                            |
| **Core**        | Blockchain implementation | [`../core`](../core)               |
| **Web**         | Web interface             | [`../web`](../web)                 |
| **SDK**         | TypeScript SDK            | [`../sdk`](../sdk)                 |
| **Marketplace** | Compute marketplace       | [`../marketplace`](../marketplace) |
| **DevTools**    | Development tools         | [`../devtools`](../devtools)       |
| **Deploy**      | Infrastructure            | [`../deploy`](../deploy)           |

---

## 📜 License

MIT License - see [LICENSE](LICENSE) file for details.

**Note**: The AxionAX Protocol Core uses AGPLv3. See
[`../core/LICENSE`](../core/LICENSE).

---

## 🔗 Links

- **Main Repository**: https://github.com/axionaxprotocol/axionaxiues
- **Live Documentation**: https://docs.axionax.org
- **Protocol Website**: https://axionax.org
- **Issues**: https://github.com/axionaxprotocol/axionaxiues/issues

---

**Part of the AxionAX Protocol Ecosystem**

**Last Updated**: November 6, 2025
