# Nuclear Policy Brief (OpenClaw Skill)

This skill generates a concise nuclear energy policy brief grounded in recent news. It uses Apify to pull live sources and Contextual to synthesize a structured brief.

## Quickstart

```bash
export APIFY_API_TOKEN=your_apify_token
export CONTEXTUAL_API_KEY=your_contextual_key

python3 bin/nuclear_brief.py \
  --query "nuclear energy policy OR nuclear regulation" \
  --topic "Global nuclear energy policy" \
  --audience "policy analysts" \
  --country "US" \
  --timeframe "7d"
```

## Demo Tips
- Show the Apify run (live data) and point at one or two sources.
- Show the brief output and the Sources section.
- Emphasize that the workflow is grounded in real material.
