# searxng-search

English | [中文说明](README.zh-CN.md)

An OpenClaw skill for using a SearXNG instance as a privacy-friendly web search backend.

## What this is

This repository does **not** bundle or embed the full SearXNG server.
It provides an **OpenClaw skill** that teaches the agent when and how to query a SearXNG instance over HTTP.

## What it does

- Explain when to use SearXNG instead of Brave or other search backends
- Prefer JSON search responses from `/search`
- Automatically try healthy public SearXNG instances if no instance is provided
- Fall back to HTML search results when JSON is rate-limited or disabled
- For version/release queries, prefer official sources like `site:github.com` or the vendor domain
- Provide a minimal, repeatable search workflow
- Document common API parameters such as:
  - `q`
  - `format=json`
  - `language`
  - `time_range`
  - `engines`
  - `categories`
  - `safesearch`

## What it does not do

- It does **not** install SearXNG itself
- It does **not** run a search server
- It does **not** magically add Brave Search inside SearXNG

You still need a working SearXNG instance URL.

## Files

- `SKILL.md` — main skill instructions
- `references/api.md` — compact SearXNG API reference

## Install

### Option 1: Copy into your OpenClaw workspace

Place this folder at:

```text
~/.openclaw/workspace/skills/searxng-search/
```

Then start a new OpenClaw session.

### Option 2: Use the packaged artifact

If you package the skill into a `.skill` archive, import or unpack it into your workspace skills directory.

## Usage idea

Tell OpenClaw things like:

- "Use my SearXNG instance to search for recent OpenClaw releases"
- "Search via SearXNG for privacy-focused metasearch engines"
- "Use SearXNG JSON API and summarize the top 5 results"

## Notes

SearXNG is a metasearch engine framework. Public instances may differ in:

- enabled engines
- JSON support
- rate limits
- categories and plugins

For reliable automation, prefer your own instance.

## License

This skill content is provided as-is for OpenClaw users.
