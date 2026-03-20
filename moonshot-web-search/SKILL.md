---
name: moonshot-web-search
description: Search the live web through Moonshot's builtin $web_search tool and return a concise answer. Use when the user asks to search the web, look something up online, verify current information, or summarize recent web findings with Moonshot. Use this for fast web-backed answers when direct browser-style source extraction is not required.
---

# Moonshot Web Search

Run a bundled shell script that calls Moonshot's builtin `"$web_search"` tool in two rounds and prints the final answer.

## Resources

- `scripts/search.sh`: Execute a Moonshot web search and print the model's final answer.

## Workflow

1. Resolve `SKILL_DIR` as the directory containing this `SKILL.md`.
2. Check that `MOONSHOT_API_KEY` is set before running the script. If it is missing, tell the user they need to provide a valid Moonshot API key in the environment.
3. Extract a clean search request from the user's prompt. Preserve concrete constraints such as time range, geography, language, and output format.
4. Run:

```bash
bash "${SKILL_DIR}/scripts/search.sh" "<query>"
```

5. Return the script output to the user in the requested style. If the user asked for a short answer, compress it after reading the result.

## Output Expectations

- Expect plain text output only.
- Do not claim that you personally fetched URLs or inspected source pages unless you actually did so with other tools.
- If the user explicitly needs citations, exact links, or page-level attribution, prefer a browser/search workflow instead of relying only on this skill.

## Troubleshooting

- If the script reports `请设置 MOONSHOT_API_KEY 环境变量`, stop and ask the user to configure the key.
- If `curl` returns HTTP `429`, tell the user the Moonshot account is being rate limited and retry later.
- If the first round does not return `tool_calls`, surface the error text from the script.
- If the second round returns no content, report the script error rather than guessing.
