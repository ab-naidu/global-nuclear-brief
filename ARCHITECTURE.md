# Architecture Overview — Global Nuclear Brief

This document explains how the OpenClaw skill works end to end, where the key logic lives, and how to modify it.

## 1) Entry point
The skill is executed via:
```bash
python3 bin/nuclear_brief.py ...
```

The OpenClaw runtime reads `SKILL.md`, which points to this script and documents required config.

## 2) Configuration and secrets
The script reads keys from environment variables:
- `APIFY_API_TOKEN`
- `CONTEXTUAL_API_KEY`

It also loads a local `.env` file (if present) from the repo root.

Relevant code:
- `bin/nuclear_brief.py` → `load_env_file()` and `os.getenv(...)`

## 3) Data flow (system loop)
1. **Input**: CLI args like `--query`, `--country`, `--timeframe`.
2. **Retrieve**: Apify actor fetches live news.
3. **Reason**: Contextual produces a grounded brief using retrieved items.
4. **Output**: A structured brief printed to stdout.

## 4) Apify integration
The script calls Apify’s `run-sync-get-dataset-items` endpoint with a Google News actor.

Key logic:
- `fetch_news()` builds the actor input payload.
- Actor ID is normalized to `owner~actor` format.

Files and functions:
- `bin/nuclear_brief.py` → `fetch_news()`

## 5) Normalization and knowledge packaging
Raw Apify results are normalized into a simple structure:
- `title`, `link`, `source`, `date`, `snippet`

Then the script builds a `knowledge` list (one item per article) to send to Contextual.

Files and functions:
- `bin/nuclear_brief.py` → `normalize_article()`, `build_knowledge()`

## 6) Contextual generation
The script calls Contextual’s `/generate` endpoint with:
- A system prompt to enforce grounding
- A user prompt that enforces the output structure
- The `knowledge` list from Apify

Files and functions:
- `bin/nuclear_brief.py` → `generate_brief()`

## 7) Output cleanup
Contextual output can include XML-like tags or empty link markers. The script cleans those before printing.

Files and functions:
- `bin/nuclear_brief.py` → `clean_output()`

## 8) Key files
- `bin/nuclear_brief.py`: main skill logic
- `SKILL.md`: OpenClaw skill metadata and run instructions
- `README.md`: user-facing documentation
- `DEMO.md`: 3-minute demo script
- `PROJECT.md`: hackathon submission template
- `.env.example`: configuration template

## 9) How to extend
- Add **region presets** by mapping `--region` to country codes.
- Add **daily diff** by storing prior briefs (Redis) and comparing.
- Add **alert thresholds** by scanning the brief for key terms.

## 10) Common issues
- **Missing API key**: set `APIFY_API_TOKEN` and `CONTEXTUAL_API_KEY`.
- **Apify 404**: ensure the actor id uses `owner~actor` format.
- **No results**: widen the query or timeframe.
