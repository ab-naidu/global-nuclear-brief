# Global Nuclear Brief (OpenClaw Skill)

Global Nuclear Brief is an OpenClaw skill that monitors recent nuclear energy policy and regulatory developments and produces a concise, decision ready brief for analysts. It turns live public information into grounded, actionable summaries so teams can quickly understand what changed, why it matters, and what to do next.

Built at OpenClaw Hack Day (San Francisco).

## Main idea
Policy shifts in nuclear energy are global and fast. Manually tracking updates across scattered sources is slow and error prone. This skill automates the monitoring and synthesizes the signal into a structured brief that can be read in minutes and acted on immediately.

## Who this is for
- Energy policy analysts and research teams
- Utilities and operations teams tracking regulatory change
- Journalists and educators who need grounded, source backed summaries

## What it does
- Pulls live nuclear policy and regulatory news via Apify.
- Synthesizes a structured brief using Contextual, grounded only in retrieved sources.
- Outputs actionable sections for fast review with sources attached.

## How it works (system loop)
1. **Input**: a query, region, and timeframe (for example: “nuclear energy policy”, US, last 7 days)
2. **Retrieve**: Apify pulls live results from Google News
3. **Reason**: Contextual generates a structured brief using only the retrieved sources
4. **Output**: a decision ready brief with sources

## Output format
1. Top changes (3-5 bullets)
2. Why it matters (3 bullets)
3. Risks and uncertainties (2-3 bullets)
4. Recommended actions (3 bullets)
5. Sources (titles + links)

## Quickstart
```bash
# Create a local .env file with your keys (never commit this file)
cat > .env <<'ENV'
APIFY_API_TOKEN=your_apify_token
CONTEXTUAL_API_KEY=your_contextual_key
CONTEXTUAL_MODEL=v2
ENV

# Run the skill
python3 bin/nuclear_brief.py \
  --query "nuclear energy policy OR nuclear regulation" \
  --topic "Global nuclear energy policy" \
  --audience "policy analysts" \
  --country "US" \
  --timeframe "7d"
```

## Configuration
- `APIFY_API_TOKEN` (required): Apify API token
- `CONTEXTUAL_API_KEY` (required): Contextual API key
- `CONTEXTUAL_MODEL` (optional, default: v2): model version

## CLI options
- `--query`: search query for live news
- `--topic`: brief topic focus
- `--audience`: target audience
- `--language`: language code for news
- `--country`: country code for news
- `--timeframe`: time filter (e.g., 1d, 7d, 1m)
- `--max-articles`: max articles fetched from Apify
- `--knowledge-limit`: max items sent to Contextual
- `--actor`: Apify actor id (owner~actor)

## Example runs
```bash
# EU focused brief
python3 bin/nuclear_brief.py \
  --query "nuclear energy policy OR nuclear regulation" \
  --topic "EU nuclear energy policy" \
  --audience "policy analysts" \
  --country "FR" \
  --timeframe "7d"

# Emerging market focus
python3 bin/nuclear_brief.py \
  --query "small modular reactor OR SMR policy" \
  --topic "Global SMR policy" \
  --audience "energy strategists" \
  --country "IN" \
  --timeframe "30d"
```

## Demo tips
- Show the live data pull (Apify) and one or two sources.
- Show the brief output and the Sources section.
- Emphasize that the brief is grounded in real, current material.

## Roadmap ideas
- Daily diff mode (compare today vs yesterday)
- Region presets (US, EU, India)
- Alert thresholding on major policy shifts
- Optional memory layer (Redis) for multi day tracking

## Architecture
See `ARCHITECTURE.md` for a full technical walkthrough.
