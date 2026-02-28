# Toy Store Revenue Leak Observatory - Light Build

This bundle is the deadline-safe fallback version of the main Maven Fuzzy Factory build.

It keeps the same repo-root extractable transport format as the main bundle, but reduces the problem to a **single iteration** and a very small set of DuckDB-safe models.

## What is included

- `.claude/skills/toy-store-revenue-leak-observatory-light/` - the simplified skill, contracts, IR, and prompt
- `datasets/maven-fuzzy-factory/raw/` - the extracted raw CSV files, included uncompressed
- `datasets/maven-fuzzy-factory/profiles/` - small machine-readable dataset profiles
- `datasets/maven-fuzzy-factory/manifests/` - checksums and a dataset summary

## Installation

Transport format is a single zip archive.

Runtime expectation is **unpacked directories on disk**. Extract this archive at repo root so that these two paths exist:

- `.claude/skills/toy-store-revenue-leak-observatory-light/`
- `datasets/maven-fuzzy-factory/`

If your agent can unzip files itself, you may commit the archive into the repo and have the agent extract it. The skill itself is intended to run from the unpacked filesystem layout above.

## What this light build does

It creates a small, reliable diagnostic slice of the original idea:

- session sources
- pageview sources
- order sources
- refund sources
- one session-level fact model
- one monthly segment summary
- one ranked priority-actions table

## What this light build does not try to do

To maximize first-pass success, it intentionally removes:

- multi-iteration delivery
- experiment-window logic
- page-path sequencing and funnel-step marts
- product launch and bundle analysis
- unified observatory and explainer layers

See `docs/what-was-removed.md` for details.

## Dataset baseline

- sessions: 472,871
- pageviews: 1,188,124
- orders: 32,313
- refunded order items: 1,731
- activity window: 2012-03-19 08:04:16 to 2015-04-01 18:11:08

## Recommended read order for the agent

1. `SKILL.md`
2. `phase-plan.json`
3. `contracts/source-manifest.json`
4. `datasets/maven-fuzzy-factory/profiles/table-profiles.json`
5. `prompts/iteration-01-single-pass-core-leak-summary.md`
6. `ir/stage-01.json`
