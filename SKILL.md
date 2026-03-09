---
name: searxng-search
description: Query a SearXNG instance as a replacement for Brave or other web-search backends. Use when the user wants privacy-friendly web search, self-hosted/meta-search, or asks to search through a SearXNG instance via HTTP API endpoints such as /search with JSON output.
---

# SearXNG Search

Use SearXNG when web search should go through a SearXNG instance instead of Brave, Google, or another provider. Prefer JSON output and keep requests simple and reproducible.

## Quick Start

1. Confirm the SearXNG base URL or ask for it if missing.
2. Call the search endpoint with `format=json`.
3. Start with only `q` plus a small number of filters.
4. Summarize top results with titles, URLs, and short notes.
5. If needed, refine with engine, category, language, or time filters.

Basic pattern:

```text
GET {baseUrl}/search?q={query}&format=json
```

Example:

```text
GET https://searx.example.org/search?q=openclaw&format=json
```

## Recommended Workflow

### 1. Gather the minimum inputs

Need:
- `baseUrl` of the SearXNG instance
- search query

Optional:
- `language`
- `time_range=day|month|year`
- `engines`
- `categories`
- `safesearch`
- page number

If the instance is unknown, ask for the instance URL or use a documented local default if the environment already defines one.

### 2. Prefer JSON results

Use `format=json` whenever the goal is machine-readable search output.

Example query template:

```text
{baseUrl}/search?q={query}&format=json&language=en&time_range=month
```

Do not assume every instance enables JSON/CSV/RSS. If the instance returns 403 or ignores `format=json`, explain that the instance disabled that output format and fall back to HTML browsing if appropriate.

### 3. Keep filters conservative

Start broad, then narrow only when useful.

Good first refinements:
- `language` for language-specific queries
- `time_range` for recent-news style requests
- `categories` when the user wants images, news, videos, etc.
- `engines` only when the user explicitly cares about the source or the instance mixes uneven-quality engines

### 4. Report results cleanly

For each high-value result, include:
- title
- URL
- one-line relevance note

If the result set looks weak, say so and try a revised query.

## Common API Parameters

See `references/api.md` for the concise parameter reference and examples.

Most useful parameters in practice:
- `q` - required query string
- `format=json` - machine-readable output
- `language`
- `time_range`
- `engines`
- `categories`
- `pageno`
- `safesearch`

## Failure Handling

- If the instance is unreachable: report connectivity failure and ask for another instance or permission to troubleshoot.
- If JSON output is disabled: explain the limitation clearly.
- If results are noisy: refine query terms before piling on many filters.
- If the user wants a stable OpenClaw integration: recommend wiring the instance into a dedicated search tool or gateway config rather than manually repeating raw HTTP calls.

## Notes

- SearXNG is a metasearch engine, not a general hosted search API with guaranteed schema stability across all public instances.
- Public instances may differ in enabled engines, output formats, rate limits, and safesearch behavior.
- For repeatable automation, prefer a self-hosted or trusted instance.
