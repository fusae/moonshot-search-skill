# Moonshot Search Skill

GitHub-hosted skill repo for installing `moonshot-web-search` into OpenClaw or Codex-style skill runners.

## What This Repo Contains

- `moonshot-web-search/`: the installable skill directory
- `moonshot-web-search/SKILL.md`: skill instructions and trigger description
- `moonshot-web-search/scripts/search.sh`: bundled shell script that calls Moonshot's builtin `"$web_search"` tool
- `search.sh`: the standalone development copy used while building the skill

## What The Skill Does

`moonshot-web-search` runs a two-round Moonshot API flow:

1. Trigger builtin web search with `"$web_search"`
2. Feed the returned search result handle back to Moonshot
3. Print the final plain-text answer

This is intended for fast, web-backed answers when you want Moonshot to do the online lookup and summarization.

## Requirements

- A valid `MOONSHOT_API_KEY`
- `bash`
- `curl`
- `python3`

## Install Into OpenClaw / Codex

Install from GitHub repo path:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo fusae/moonshot-search-skill \
  --path moonshot-web-search
```

After installation, restart the app so the new skill is picked up.

## Environment Variable

Export your Moonshot key before using the skill:

```bash
export MOONSHOT_API_KEY="your-api-key"
```

## OpenClaw Routing Recommendation

If OpenClaw sometimes uses its own builtin `web_search` instead of this skill, add a routing rule to OpenClaw's own `TOOLS.md` so live web search prefers `moonshot-web-search` and does not call builtin `web_search` directly.

Suggested wording:

```md
For live web search, current-information lookup, and online verification, always prefer the `moonshot-web-search` skill. Do not call builtin `web_search` directly for those tasks.
```

## Local Test

Run the bundled script directly:

```bash
bash moonshot-web-search/scripts/search.sh "Moonshot AI 是什么？请用3点简述"
```

Or use the root development script:

```bash
./search.sh "今天的 AI 新闻有哪些？"
```

## Notes

- Output is plain text, not structured JSON.
- The skill is good for quick search-and-summarize tasks.
- If you need strict citations or page-level source attribution, use a browser-based workflow instead of relying only on this skill.
- If Moonshot returns HTTP `429`, wait and retry later.
