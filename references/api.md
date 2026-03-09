# SearXNG API Reference

## Endpoint

Supported by SearXNG search API:

- `GET /search`
- `POST /search`
- many instances also accept `/`

Preferred pattern:

```text
GET {baseUrl}/search?q={query}&format=json
```

## Core Parameters

### Required

- `q`: search query

### Common optional parameters

- `format`: `json`, `csv`, or `rss` if enabled by the instance
- `language`: language code
- `time_range`: `day`, `month`, `year`
- `pageno`: page number
- `safesearch`: `0`, `1`, `2`
- `categories`: comma-separated categories
- `engines`: comma-separated engine names
- `enabled_plugins`
- `disabled_plugins`
- `enabled_engines`
- `disabled_engines`

## Example Requests

### Basic JSON search

```text
GET https://searx.example.org/search?q=openclaw&format=json
```

### Recent results in English

```text
GET https://searx.example.org/search?q=openclaw&format=json&language=en&time_range=month
```

### Restrict to selected engines

```text
GET https://searx.example.org/search?q=privacy+search&format=json&engines=duckduckgo,startpage
```

### Images category

```text
GET https://searx.example.org/search?q=red+panda&format=json&categories=images
```

## Instance Caveats

- Some public instances disable JSON output.
- Supported engines differ by instance.
- Categories and plugins can vary.
- Public instances may rate-limit or block repeated automation.

## Practical Guidance

- Start with only `q` and `format=json`.
- Add `language` or `time_range` before engine-level tuning.
- Use a self-hosted instance for reliable automation.
