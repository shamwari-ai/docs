# Shamwari Docs

> The public documentation for Shamwari AI — what it does, and which parts of it are deliberately impossible.

[![CI](https://github.com/shamwari-ai/docs/actions/workflows/ci.yml/badge.svg)](https://github.com/shamwari-ai/docs/actions/workflows/ci.yml)
[![Lint](https://github.com/shamwari-ai/docs/actions/workflows/lint.yml/badge.svg)](https://github.com/shamwari-ai/docs/actions/workflows/lint.yml)
![Mintlify](https://img.shields.io/badge/Mintlify-0D9373?style=flat-square&logo=mintlify&logoColor=white)
![MDX](https://img.shields.io/badge/MDX-1B1F24?style=flat-square&logo=mdx&logoColor=white)

**Live:** [docs.shamwari.ai](https://docs.shamwari.ai) | **Product:** [shamwari.ai](https://shamwari.ai) | **Source:** [shamwari-ai/shamwari](https://github.com/shamwari-ai/shamwari)

---

## What it is

This repo is the Mintlify source for [docs.shamwari.ai](https://docs.shamwari.ai).
Eight MDX pages, `docs.json`, one stylesheet, three images. There is no
application here and no build step to run in CI beyond Mintlify's own
validator.

The pages were migrated out of `docs-site/` in the
[`shamwari`](https://github.com/shamwari-ai/shamwari) monorepo, which was an
Astro site serving the same content. `docs-site/` still exists there as a
stale mirror; this repo is the one that is deployed, and the one to edit.

The audience is someone deciding whether to trust Shamwari, not someone about
to contribute to it — the monorepo's `README.md` and `CLAUDE.md` serve that
second reader. Everything asserted on these pages is meant to be true of the
code as committed. If a claim here stops matching the implementation, the page
is the bug.

| Page               | What it covers                                                              |
| ------------------ | --------------------------------------------------------------------------- |
| `index.mdx`        | Overview — an AI companion that refuses to send your data away              |
| `rules.mdx`        | The two rules that must not be broken, and where each is enforced           |
| `scopes.mdx`       | `personal`, `community`, `platform` — three data scopes, not three tiers    |
| `architecture.mdx` | The edge Worker and the Core service, and why the split falls where it does |
| `routing.mdx`      | How a request's tier is chosen, and which model serves it                   |
| `ground.mdx`       | Retrieval, citation and effective dates                                     |
| `language.mdx`     | How Shamwari is described, and the phrasings that are not allowed           |
| `traffic.mdx`      | Six concurrent requests, and the three different places they stop           |

## Commands

| Command                                               | Description                                             |
| ----------------------------------------------------- | ------------------------------------------------------- |
| `npm i -g mint`                                       | Install the Mintlify CLI                                |
| `mint dev`                                            | Local preview on `http://localhost:3000`                |
| `mint validate`                                       | Strict validation — the CI build gate                   |
| `mint broken-links --check-anchors --check-redirects` | Internal links, anchors and `docs.json` redirects       |
| `mint a11y`                                           | Alt attributes across the MDX files                     |
| `node scripts/check-contrast.mjs`                     | APCA 3.0 contrast gate on the `docs.json` brand colours |

## What CI checks

Five jobs, and three of them exist because a generic docs check was not good
enough on its own.

- **Build validation** — `mint validate` in strict mode, so a warning fails
  the PR rather than landing quietly.
- **Broken links** — internal links, anchors and redirect destinations.
  `--check-external` is deliberately omitted: it makes a green PR depend on
  third-party site availability.
- **Accessibility** — brand colours are gated by `scripts/check-contrast.mjs`
  (APCA 3.0), not by `mint a11y`'s WCAG heuristic. That heuristic is not
  polarity-aware — it tests `colors.dark` against both backgrounds, though
  `dark` only ever renders in light mode — and it masks real failures:
  sodalite's `#3D5AFE` scores 3.78:1 against the dark background, a WCAG
  pass, at APCA Lc -26.9, below even the non-text floor. `mint a11y` still
  runs, for the alt-attribute checks it is genuinely good at.
- **Language discipline** — a grep for "open source model", "we built our own
  model" and "your data stays in Africa". `language.mdx` is excluded because
  it quotes those phrases as the ones not to use. The rule is stated in the
  monorepo's `CLAUDE.md`; here it is enforced mechanically rather than from
  memory.
- **Secret scan** — delegated to the org's
  [`reusable-gitleaks.yml`](https://github.com/shamwari-ai/.github/blob/main/.github/workflows/reusable-gitleaks.yml).

## Deploying

Mintlify's GitHub app deploys the default branch automatically. There is no
deploy step in this repo's workflows and no wrangler config — unlike every
other Shamwari surface, this one is not on Cloudflare Workers.

## Ecosystem

- [`shamwari`](https://github.com/shamwari-ai/shamwari) — the monorepo these
  pages describe; `CLAUDE.md` is the authoritative account of the two rules
- [`shamwari-gateway`](https://github.com/shamwari-ai/shamwari-gateway) and
  [`shamwari-core`](https://github.com/shamwari-ai/shamwari-core) — the two
  services `architecture.mdx` draws
- [Org standards](https://github.com/shamwari-ai/.github/blob/main/ORG_STANDARDS.md)
  — what CI runs across the org

## Licence

`LICENSE` in this repo is still the MIT licence that shipped with the Mintlify
starter kit, and it names Mintlify as the copyright holder. It has not been
replaced; treat it as covering the starter scaffolding rather than the writing.

The documentation content is © Bundu Foundation. Shamwari is Bundu Foundation
IP, sold commercially under Nyuchi Africa.
