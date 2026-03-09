# searxng-search

[English](README.md) | 中文说明

这是一个给 **OpenClaw** 使用的 skill，用来指导代理通过 **SearXNG 实例** 完成隐私友好的网页搜索。

**版本：** `v0.2.1`

**默认工作流：** `SearXNG discovery + official-page verification`

## 这是什么

这个仓库 **不包含** 完整的 SearXNG 服务端本体。  
它提供的是一个 **OpenClaw skill**，作用是告诉代理：**什么时候该用 SearXNG，以及怎样通过 HTTP API 去调用它。**

它的目标工作方式是：
- 先用 SearXNG 做结果发现
- 对高价值事实问题优先偏向官方域名
- 再打开官方页面核实后回答

## 它能做什么

- 说明什么时候适合用 SearXNG 替代 Brave 等搜索后端
- 优先使用 `/search` 的 JSON 输出
- 如果用户没有提供实例，自动尝试健康的公开 SearXNG 实例
- 如果 JSON 被限流或禁用，自动回退到 HTML 搜索结果
- 对“版本 / release / changelog”类问题，优先限定 `site:github.com` 或官方域名
- 提供一个尽量稳定、可复用的搜索流程
- 记录常用 API 参数，例如：
  - `q`
  - `format=json`
  - `language`
  - `time_range`
  - `engines`
  - `categories`
  - `safesearch`

## 它不能做什么

- **不会** 自动安装 SearXNG 服务端
- **不会** 自己运行一个搜索引擎
- **不会** 自动把 Brave Search “内嵌进” SearXNG

你仍然需要一个可用的 **SearXNG 实例地址**。

## 文件说明

- `SKILL.md` — skill 主说明
- `references/api.md` — 精简版 SearXNG API 参考

## 安装方式

### 方式 1：复制到 OpenClaw 工作区

把这个目录放到：

```text
~/.openclaw/workspace/skills/searxng-search/
```

然后重新开启一个新的 OpenClaw session。

### 方式 2：使用打包后的 `.skill` 文件

如果你把它打包成 `.skill` 文件，可以导入或解压到工作区的 `skills/` 目录下。

## 使用示例

你可以对 OpenClaw 说：

- “用我的 SearXNG 实例搜索最新的 OpenClaw release”
- “通过 SearXNG 搜一下隐私友好的元搜索引擎”
- “使用 SearXNG 的 JSON API，帮我总结前 5 个结果”

## 备注

SearXNG 是一个元搜索引擎框架。不同公开实例在以下方面可能不一样：

- 启用的搜索源
- 是否支持 JSON
- 限流策略
- categories 和 plugins

如果你想稳定自动化使用，建议优先使用你自己的实例。

## License

这个 skill 以当前形式提供给 OpenClaw 用户使用。
