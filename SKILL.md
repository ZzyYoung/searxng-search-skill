---
name: searxng-search
description: Query a SearXNG instance as a replacement for Brave or other web-search backends. Use when the user wants privacy-friendly web search, self-hosted/meta-search, or asks to search through a SearXNG instance via HTTP API endpoints such as /search with JSON output. If no instance is provided, try healthy public SearXNG instances automatically before bothering the user.
---

# SearXNG Search

Use SearXNG when web search should go through a SearXNG instance instead of Brave, Google, or another provider. Prefer JSON output when it works, but do not get stuck on it.

## Quick Start

1. Use the user-provided SearXNG base URL if available.
2. If no instance is provided, automatically try healthy public SearXNG instances.
3. Prefer `format=json` first.
4. If JSON is blocked, rate-limited, or disabled, fall back to HTML search results.
5. Summarize the best results with titles, URLs, and short notes.
6. For version/release queries, bias toward official sources.

Basic pattern:

```text
GET {baseUrl}/search?q={query}&format=json
```

Example:

```text
GET https://searx.example.org/search?q=openclaw&format=json
```

## Instance Selection Workflow

### 1. Prefer explicit instances

Use these in order:
- user-provided instance
- documented local default in the environment
- healthy public instances discovered from a public instance list such as `searx.space`

Do not stop just because the user did not provide an instance.

### 2. Auto-try public instances when needed

When no instance is configured, automatically try a small number of public HTTPS instances.

Preferred behavior:
- use normal HTTPS instances
- prefer recent SearXNG versions
- avoid long retry loops
- try only a few candidates before switching strategy
- if JSON endpoints return `429`, `403`, or transport failures, try another instance or use HTML mode

### 3. Prefer fewer, smarter attempts

Do not hammer public instances.

Good default sequence:
1. try JSON search on 2-4 candidates
2. if rate-limited or blocked, open one candidate in the browser and use HTML search
3. continue with the best available result source

Only tell the user about instance-selection details if all attempts fail or if the user explicitly asks.

## Query Construction

### Normal search

Start simple:
- query only
- `format=json`
- optionally `language`
- optionally `time_range`

### Version and release queries

For version checks, latest release questions, download versions, changelogs, tags, or package-release lookups, prefer official or authoritative sources.

Refine the query before searching. Examples:

```text
zerotier latest version site:github.com
zerotier latest release site:github.com
openclaw release notes site:github.com
zerotier 1.16.0 site:zerotier.com OR site:github.com
```

Bias order for version queries:
1. official GitHub releases/tags
2. official project site or docs
3. package registries or vendor docs
4. blogs and mirrors only as fallback

If SearXNG returns noisy tutorial/blog results for a version query, rewrite the query with `site:github.com` or the official domain and search again without asking first.

## JSON vs HTML Strategy

### Prefer JSON first

Use `format=json` whenever the goal is machine-readable search output.

Example:

```text
{baseUrl}/search?q={query}&format=json&language=en&time_range=month
```

### Fall back to HTML without drama

Do not assume every instance enables JSON/CSV/RSS. If JSON is disabled or rate-limited:
- switch to HTML search results on the same or another instance
- use browser automation to inspect top results
- keep the user-facing answer focused on the result, not the transport details

## Result Evaluation

For each strong result, capture:
- title
- URL
- one-line relevance note

For version queries, prefer results that visibly contain:
- `releases/tag/...`
- `releases/latest`
- official changelog pages
- official download pages
- explicit version strings near the project name

If the result set looks weak, do not present low-confidence guesses as facts. Rewrite and retry with narrower official-source filters.

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

- If one public instance is unreachable: silently try another.
- If JSON output is disabled: switch to HTML mode.
- If public instances return `429`: reduce attempts and switch strategy instead of looping.
- If results are noisy: refine query terms before piling on many filters.
- If all public-instance attempts fail: then tell the user you need a specific instance or another source.
- If the user wants stable long-term use: recommend a self-hosted or trusted instance.

## Notes

- SearXNG is a metasearch engine, not a general hosted search API with guaranteed schema stability across all public instances.
- Public instances differ in enabled engines, output formats, rate limits, and safesearch behavior.
- For repeatable automation, prefer a self-hosted or trusted instance.
