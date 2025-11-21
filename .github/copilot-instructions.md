# Axionax Docs - Copilot Instructions
# Target Model: Gemini 2.5 Pro

**Context:** Documentation Site (Jekyll, Liquid, Markdown).

## 📝 DOCUMENTATION ARCHITECTURE
1.  **Content Consistency:**
    - Cross-reference parameters with `axionax-core` code.
    - Terminology: "Validator" (not Miner), "Worker" (Compute Provider).

2.  **Jekyll & Liquid:**
    - Use strict Front Matter (YAML) format.
    - Use Liquid tags `{% include %}` for reusable components.
    - Use relative paths for internal links.

3.  **SEO & Structure:**
    - Start pages with benefit-driven summaries.
    - Use clear H2/H3 hierarchy.
    - Add `alt` text to images.

4.  **Mermaid Diagrams:**
    - Suggest Mermaid.js for explaining complex flows.
