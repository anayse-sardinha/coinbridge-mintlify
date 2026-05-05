# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Project Is

A Mintlify documentation site for **CoinBridge** — a fintech platform that turns loyalty points into universal payment instruments. The repo is pure documentation content (MDX files + config); Mintlify handles build and hosting automatically on push to `main`.

## Local Development

Requires Node.js v19+.

```bash
# Install Mintlify CLI (once)
npm i -g mint

# Start local preview
mint dev

# Validate all links
mint broken-links

# Update CLI
mint update
```

Preview runs at `http://localhost:3000` by default. Use `--port <number>` to change ports.

Deployment is automatic: pushing to `main` triggers production deployment via the Mintlify GitHub App.

## Architecture

### Content Structure

- **`docs/`** — all documentation pages as `.mdx` files, organized by topic:
  - `sdk/` — unified SDK integration guides (Android and iOS, separated by tabs within pages)
  - `webhooks/endpoints/` — Backend Connector webhook endpoint docs
  - `payments/` — payment operations (transactions, refunds, corrections)
  - `coinbridge-api/` — API reference pages
  - `openapi.json` — OpenAPI 3.0.3 spec for the Backend Connector API (rendered as interactive playground)
- **`snippets/`** — reusable MDX fragments included across multiple pages
- **`images/`** and **`logo/`** — static assets

### Site Configuration

All navigation, theming, versioning, and API settings live in **`docs.json`** (Mintlify v1.37 config format). This is the single file controlling the entire site structure. Key areas:

- `navigation` — tab layout with three tabs: Home, Guides (SDK + Backend Connector anchors), API
- `api.playground` — set to `"simple"` for the OpenAPI interactive display
- `versions` — version tabs are defined here if multi-version docs are needed

### OpenAPI Integration

`docs/openapi.json` is the OpenAPI 3.0.3 spec (Backend Connector API v2.1.0). Mintlify auto-generates interactive API reference pages from it. The spec covers:
- Payment Operations
- Refunds & Corrections
- User Events (webhooks)

When updating the API spec, regenerate or hand-edit `docs/openapi.json` and the navigation entries in `docs.json` will pick up the changes automatically on the next `mint dev` restart.

## Local Research Resources

> These folders are gitignored — they exist only on the local machine and are not committed.

### Meeting transcripts — `resources/transcripts/`

Raw meeting files (`.docx` or `.md`) from stakeholder calls. Drop new transcripts here before processing.

Use `/summarize-transcript <filename>` to extract decisions, action items, and open questions. Summaries are saved to `resources/transcripts/summaries/` and should be consulted before writing or editing any documentation page.

| File | Covers |
|------|--------|
| `CoinBridge-Kick-Off-2-87773722-9369.docx` | **Kickoff call (iteration 2)** — unify Android/iOS into one doc with tabs, move webhooks under SDK, simplify SDK to 3 steps (init, issue card, add to wallet), add high-level system diagram, integration journey structure, Nayax link strategy, work order (structure → content → API → home page last) |

## Content Conventions

- Pages are `.mdx` (Markdown + JSX). Mintlify components like `<Card>`, `<CardGroup>`, `<Steps>`, `<CodeGroup>` are used extensively.
- Shared content goes in `snippets/` and is referenced with `<Snippet file="name.mdx" />`.
- Front matter fields used across pages: `title`, `description`, `sidebarTitle`, `icon`.
- The homepage is `index.mdx`; the development guide is `development.mdx`.
- **Never use em dashes** (`—`) in any documentation content or generated text. Use a comma, period, or rewrite the sentence instead.

### Component Usage

- **`<Steps>`** — Use for any sequential process. Never use manual `### Step N:` headings or ordered markdown lists for flows.
- **`<Tabs>`** — Use to separate Android and iOS content within a shared page.
- **`<Accordion>`** — Use for "I might need this" reference content (field tables, payload schemas). Do not use for content the reader must see to proceed.
- **`<Warning>`** — Use for mandatory rules and destructive consequences. Never use `<Note>**Warning:**...` as a substitute.
- **`<Note>`** — Use for helpful context. Remove any Note that repeats information already stated in the intro paragraph.
- **Platform badges** — Any `##` or `###` section outside of a `<Tab>` that applies to one platform only must include a badge in the heading: `<Badge icon="apple" color="blue">iOS only</Badge>` or `<Badge icon="android" color="green">Android only</Badge>`.

### Writing Rules

- Remove redundant intro sentences before `<Steps>` or lists ("The following steps outline...", "Here is the process:").
- Avoid jargon: never use "state-management", "correlate", "definitive state", "drive your UI logic", or "fast handshake". Say what the thing actually does.
- Always put the explanation before a `<Warning>`. Never open a warning with "this handling" if the handling hasn't been described yet.
- Keep platform-specific placement details (e.g. "call in `onCreate()`") inside the relevant `<Tab>`, not in shared content above the tabs.
- Inside `<Accordion>` blocks, show the code/JSON example first, then the field reference table, then any warnings.

### Content Review

Run `/docs-reviewer` to check any page (or all pages) against these conventions before committing.
