# Axionax Documentation

Static documentation site for the Axionax protocol.

## Overview

This repository contains the markdown, HTML, and assets that power [docs.axionax.org](https://docs.axionax.org).

- `index.md` / `index.html` - Landing page
- `ARCHITECTURE.md`, `STATE_RPC_USAGE.md`, etc. - Detailed guides
- `_includes/`, `assets/` - Jekyll theme components
- `CNAME` - Custom domain configuration

## Local Development

```bash
bundle install
bundle exec jekyll serve
```

The site will be available at `http://localhost:4000`.

## Deployment

GitHub Pages builds from the `main` branch. Push changes to publish updates.

## License

MIT License
