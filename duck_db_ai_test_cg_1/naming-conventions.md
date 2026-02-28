---
name: toy-store-revenue-leak-observatory-light
description: Single-iteration, simplified Prophecy pipeline build for the Maven Fuzzy Factory dataset. Use when delivery certainty matters more than analytical breadth. Builds a small DuckDB-safe core leak summary and ranked priority-actions table over local CSVs.
argument-hint: [build the single-pass fallback]
---

# Toy Store Revenue Leak Observatory - Light Build

You are building a **single-iteration** Prophecy pipeline over a local DuckDB-friendly CSV dataset.

## Mission

Build the smallest useful version of the revenue leak idea over the Maven Fuzzy Factory data.

The goal is to produce a reliable first-pass pipeline that answers:

- which traffic and landing-page segments convert poorly,
- which segments produce weak net revenue per session,
- which segments show unusually high refund drag,
- and which segments should be fixed first.

This light build is intentionally narrower than the main build. Reliability beats breadth.

## Non-negotiable rules

1. **DuckDB only**
   - Use DuckDB-compatible SQL patterns only.
   - Do not emit Spark, Databricks, BigQuery, Snowflake, or warehouse-specific syntax unless it is also valid in DuckDB.

2. **Single iteration only**
   - Do not invent additional stages.
   - Build only the models listed in `contracts/model-registry.json` and `ir/stage-01.json`.

3. **Read from disk before reasoning from memory**
   - Read `phase-plan.json`, `contracts/source-manifest.json`, `contracts/model-registry.json`, `contracts/metric-catalog.json`, `contracts/join-contracts.json`, and the dataset profile JSON files before generating code.
   - Trust the on-disk resources over memory.

4. **No silent assumptions**
   - Do not invent columns, joins, or business rules that are not documented locally.
   - If an ambiguity remains after reading the local files, ask explicitly instead of guessing.

5. **Use generated-model refs**
   - Whenever one generated model depends on another generated model, reference the upstream model using:
     `{{ ref('model_name') }}`
   - Do not use `{{ source() }}` for internally generated models.

6. **Keep the build intentionally small**
   - Do not add experiment windows.
   - Do not add funnel-step sequencing.
   - Do not add launch analysis.
   - Do not add bundle analysis.
   - Do not build extra marts because they seem interesting.

7. **Normalize null traffic values**
   - Normalize null `utm_source` and `utm_campaign` to `direct_or_unknown`.
   - Normalize null `device_type` to `unknown`.

8. **Use month grain**
   - Derive `session_month` with `date_trunc('month', session_created_at)`.
   - The summary mart must aggregate at month grain, not day grain.

## Build algorithm

1. Read `phase-plan.json`.
2. Read `prompts/iteration-01-single-pass-core-leak-summary.md`.
3. Read `ir/stage-01.json`.
4. Build only the required models in the specified order.
5. Validate against `contracts/acceptance-tests.json`.
6. Report the models created, refs used, and tests satisfied.

## Required local dataset

The raw dataset is already included under `datasets/maven-fuzzy-factory/raw/`.

Required in the light build:
- `website_sessions.csv`
- `website_pageviews.csv`
- `orders.csv`
- `order_item_refunds.csv`

Included but intentionally not required in the light build:
- `order_items.csv`
- `products.csv`
- `maven_fuzzy_factory_data_dictionary.csv`

## What “good” looks like

A good light build:
- uses the real supplied schema,
- creates a small DAG that is easy to understand,
- uses `{{ ref('model_name') }}` between generated models,
- ends with `mart__core_leak_summary` and `mart__priority_actions`,
- and avoids all unnecessary complexity.
