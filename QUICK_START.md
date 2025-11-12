# 🚀 Quick Start Guide - axionax-docs

## Overview

**axionax-docs** เป็น documentation repository สำหรับ axionax protocol ครอบคลุมทุกด้านของ protocol, SDK, และการพัฒนา dApps

**Repository:** https://github.com/axionaxprotocol/axionax-docs  
**Live Site:** https://axionaxprotocol.github.io/axionax-docs/

---

## 📋 Prerequisites

```bash
# Required
- Node.js 18+ (for building docs)
- Git
- Markdown editor

# Recommended
- VS Code with Markdown extensions
- Markdown Preview Enhanced
- Grammarly extension
```

---

## 🔧 Installation

### 1. Clone Repository

```bash
git clone https://github.com/axionaxprotocol/axionax-docs.git
cd axionax-docs
```

### 2. Install Dependencies (if using static site generator)

```bash
# If using VitePress, Docusaurus, or similar
npm install

# If using plain markdown, no installation needed
```

### 3. Preview Documentation

```bash
# If using VitePress
npm run docs:dev

# If using Docusaurus
npm run start

# If using GitHub Pages
# Just edit markdown files and push
```

---

## 📚 Documentation Structure

```
axionax-docs/
├── README.md                   # Home page
├── QUICKSTART.md               # Quick start guide
├── ROADMAP.md                  # Project roadmap
├── TESTNET_LAUNCH.md           # Testnet launch info
│
├── protocol/                   # Protocol documentation
│   ├── overview.md             # Protocol overview
│   ├── consensus.md            # Consensus mechanism (PoPC)
│   ├── networking.md           # P2P networking
│   ├── storage.md              # State & storage
│   └── smart-contracts.md      # Smart contract execution
│
├── deai/                       # DeAI documentation
│   ├── overview.md             # DeAI system overview
│   ├── models.md               # AI models
│   ├── training.md             # Distributed training
│   └── inference.md            # Inference engine
│
├── api/                        # API documentation
│   ├── rpc.md                  # JSON-RPC API
│   ├── rest.md                 # REST API
│   ├── websocket.md            # WebSocket API
│   └── graphql.md              # GraphQL API (future)
│
├── sdk/                        # SDK documentation
│   ├── typescript.md           # TypeScript SDK
│   ├── python.md               # Python SDK (future)
│   ├── rust.md                 # Rust SDK (future)
│   └── examples.md             # Code examples
│
├── dapps/                      # dApp development
│   ├── getting-started.md      # Getting started
│   ├── wallet-integration.md   # Wallet integration
│   ├── smart-contracts.md      # Smart contract development
│   └── marketplace.md          # Marketplace integration
│
├── guides/                     # Developer guides
│   ├── setup-node.md           # Node setup
│   ├── become-validator.md     # Validator guide
│   ├── deploy-contract.md      # Contract deployment
│   └── contribute.md           # Contributing guide
│
├── tutorials/                  # Step-by-step tutorials
│   ├── hello-world.md          # Hello world dApp
│   ├── token-contract.md       # Create token
│   ├── nft-marketplace.md      # NFT marketplace
│   └── defi-protocol.md        # DeFi protocol
│
└── references/                 # Reference materials
    ├── glossary.md             # Terminology
    ├── faq.md                  # FAQ
    └── resources.md            # External resources
```

---

## ✍️ Writing Documentation

### Markdown Best Practices

```markdown
# Use H1 for page title (only one per page)

## Use H2 for main sections

### Use H3 for subsections

**Bold** for emphasis
*Italic* for subtle emphasis
`code` for inline code
[link text](url) for links

\`\`\`language
// Code blocks with language syntax highlighting
const example = "code"
\`\`\`

> Blockquotes for important notes

- Unordered lists
- For bullet points

1. Ordered lists
2. For numbered steps
```

### Code Examples

````markdown
```typescript
// Always include language for syntax highlighting
import { AxionaxClient } from '@axionax/sdk'

const client = new AxionaxClient('http://localhost:8545')
const balance = await client.getBalance('0x...')
console.log('Balance:', balance)
```

```bash
# Shell commands should use bash
npm install @axionax/sdk
npm run dev
```

```solidity
// Smart contract examples
contract MyToken {
  string public name = "My Token";
}
```
````

### Adding Images

```markdown
# Store images in images/ directory
![Alt text](images/architecture-diagram.png)

# External images
![Logo](https://axionax.org/logo.png)

# With caption
<figure>
  <img src="images/consensus-flow.png" alt="Consensus Flow" />
  <figcaption>Figure 1: Consensus mechanism flow</figcaption>
</figure>
```

---

## 🔨 Common Tasks

### Add New Documentation Page

```bash
# 1. Create markdown file
touch protocol/new-feature.md

# 2. Add frontmatter (if using static site generator)
cat > protocol/new-feature.md << 'EOF'
---
title: New Feature
description: Description of the new feature
---

# New Feature

Content goes here...
EOF

# 3. Add to navigation (if using VitePress/Docusaurus)
# Edit .vitepress/config.js or sidebars.js

# 4. Preview locally
npm run docs:dev

# 5. Commit changes
git add .
git commit -m "docs: add new feature documentation [Local dev]"
git push
```

### Update Existing Documentation

```bash
# 1. Edit markdown file
code protocol/consensus.md

# 2. Preview changes
npm run docs:dev

# 3. Check for broken links
npm run docs:build

# 4. Commit changes
git add .
git commit -m "docs: update consensus mechanism docs [Local dev]"
git push
```

### Add Code Example

```bash
# 1. Create example file
mkdir -p examples/typescript
touch examples/typescript/send-transaction.ts

# 2. Write example code
cat > examples/typescript/send-transaction.ts << 'EOF'
import { AxionaxClient, Wallet } from '@axionax/sdk'

async function sendTransaction() {
  const client = new AxionaxClient('http://localhost:8545')
  const wallet = Wallet.createRandom()
  
  const tx = await client.sendTransaction({
    from: wallet.address,
    to: '0x...',
    value: '1000000000000000000'
  })
  
  console.log('Transaction hash:', tx.hash)
}
EOF

# 3. Reference in documentation
# In sdk/examples.md:
# See full example: [Send Transaction](../examples/typescript/send-transaction.ts)

# 4. Commit
git add .
git commit -m "docs: add send transaction example [Local dev]"
git push
```

---

## 🔍 Documentation Review Checklist

### Before Publishing

- [ ] **Accuracy**: All technical information is correct
- [ ] **Completeness**: Covers all necessary topics
- [ ] **Clarity**: Easy to understand for target audience
- [ ] **Code Examples**: All code examples work
- [ ] **Links**: All internal and external links work
- [ ] **Grammar**: No spelling or grammar errors
- [ ] **Formatting**: Consistent markdown formatting
- [ ] **Images**: All images load correctly
- [ ] **Navigation**: Page is accessible from navigation
- [ ] **SEO**: Title and description are set

### Testing

```bash
# Check for broken links
npm run docs:check-links

# Build documentation (catches errors)
npm run docs:build

# Preview production build
npm run docs:preview

# Spell check
npm run docs:spellcheck
```

---

## 🚀 Publishing Documentation

### GitHub Pages (Automatic)

```bash
# 1. Make changes and commit
git add .
git commit -m "docs: update API documentation [Local dev]"
git push origin main

# 2. GitHub Actions will automatically:
#    - Build documentation
#    - Deploy to GitHub Pages
#    - Available at https://axionaxprotocol.github.io/axionax-docs/
```

### Manual Deployment

```bash
# Build documentation
npm run docs:build

# Preview build
npm run docs:preview

# Deploy manually
npm run docs:deploy
```

---

## 🔌 Integration with Other Repos

### Linking to Code Examples

```markdown
# Link to specific file in axionax-sdk-ts
See implementation: [AxionaxClient](https://github.com/axionaxprotocol/axionax-sdk-ts/blob/main/src/client/index.ts)

# Link to specific line
[Transaction signing](https://github.com/axionaxprotocol/axionax-sdk-ts/blob/main/src/wallet/index.ts#L45)
```

### Embedding Code from Repos

```markdown
# Instead of copying code, link to it
\`\`\`typescript
// See full implementation:
// https://github.com/axionaxprotocol/axionax-sdk-ts/blob/main/src/client/index.ts

import { AxionaxClient } from '@axionax/sdk'
// ... simplified example
\`\`\`
```

### API Documentation from Code

```bash
# Generate API docs from TypeScript
cd ../axionax-sdk-ts
npm run docs:generate

# Copy to docs repo
cp -r docs/api ../axionax-docs/api/reference/
```

---

## 📊 Documentation Statistics

### Analyze Documentation

```bash
# Count words
find . -name "*.md" -exec wc -w {} + | sort -n

# Count files
find . -name "*.md" | wc -l

# Check documentation coverage
npm run docs:coverage

# Generate stats
npm run docs:stats
```

---

## 🎨 Styling Documentation

### Using VitePress/Docusaurus Themes

```javascript
// .vitepress/config.js or docusaurus.config.js
export default {
  themeConfig: {
    logo: '/logo.png',
    nav: [...],
    sidebar: [...],
    footer: {
      message: 'Built with ❤️ by axionax protocol Team'
    }
  }
}
```

### Custom CSS

```css
/* .vitepress/theme/custom.css */
:root {
  --vp-c-brand: #0070f3;
  --vp-c-brand-light: #52a8ff;
  --vp-c-brand-dark: #0056c7;
}

.custom-block.tip {
  border-color: var(--vp-c-brand);
}
```

---

## 🤝 Contributing to Documentation

### Contribution Workflow

```bash
# 1. Fork the repository on GitHub

# 2. Clone your fork
git clone https://github.com/YOUR_USERNAME/axionax-docs.git

# 3. Create feature branch
git checkout -b docs/improve-consensus-section

# 4. Make changes
# ... edit files ...

# 5. Preview locally
npm run docs:dev

# 6. Commit changes
git add .
git commit -m "docs: improve consensus mechanism explanation"

# 7. Push to your fork
git push origin docs/improve-consensus-section

# 8. Create Pull Request on GitHub
```

### Documentation Standards

1. **Clarity**: Write for developers of all skill levels
2. **Examples**: Include working code examples
3. **Accuracy**: Verify all technical details
4. **Structure**: Follow existing structure
5. **Links**: Link to related documentation
6. **Updates**: Keep documentation in sync with code

---

## 🚨 Troubleshooting

### Build Errors

```bash
# Clear cache
rm -rf node_modules .vitepress/cache
npm install

# Rebuild
npm run docs:build
```

### Broken Links

```bash
# Check for broken links
npm run docs:check-links

# Fix relative links
# Use absolute paths: /protocol/consensus.md
# Not relative paths: ../protocol/consensus.md
```

### Images Not Loading

```bash
# Check image path
# Store in public/ directory: /images/diagram.png
# Not in docs/images/: ../images/diagram.png

# Verify image exists
ls -la public/images/
```

---

## 📝 Configuration

### VitePress Config Example

```javascript
// .vitepress/config.js
export default {
  title: 'axionax Protocol Documentation',
  description: 'Complete guide to axionax protocol',
  base: '/axionax-docs/',
  
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: 'Guide', link: '/guides/getting-started' },
      { text: 'API', link: '/api/rpc' },
    ],
    
    sidebar: {
      '/guides/': [
        {
          text: 'Getting Started',
          items: [
            { text: 'Introduction', link: '/guides/introduction' },
            { text: 'Setup', link: '/guides/setup' }
          ]
        }
      ]
    }
  }
}
```

---

## 📚 Additional Resources

- **Markdown Guide:** https://www.markdownguide.org
- **VitePress:** https://vitepress.dev
- **Docusaurus:** https://docusaurus.io
- **Writing Style Guide:** https://developers.google.com/style

---

## 🤝 Getting Help

- **Issues:** Report issues on [GitHub Issues](https://github.com/axionaxprotocol/axionax-docs/issues)
- **Contributing:** See [CONTRIBUTING.md](CONTRIBUTING.md)
- **Style Guide:** Follow existing documentation style

---

## 📄 License

MIT - See [LICENSE](LICENSE) file for details

---

<p align="center">
  <sub>Built with ❤️ by the axionax protocol Team</sub>
</p>
