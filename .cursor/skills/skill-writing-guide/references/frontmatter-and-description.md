# Frontmatter 与 Description 详解

（本文件供 skill 正文中「详见 references/」时按需加载。）

## YAML 必填字段

```yaml
---
name: your-skill-name   # kebab-case，与文件夹名一致
description: 一句话说清做什么。Use when user asks to [具体短语]、[文件类型] 或 says [用户原话].
---
```

## Description 结构（官方推荐）

`[做什么] + [何时用] + [关键能力/文件类型]`

- 做什么：一句能概括价值的话。
- 何时用：必须写，否则 Agent 不知道何时加载；包含用户会说的词、会传的文件类型。
- 可选：关键能力或限制（如「仅限 PDF」「需先连接 MCP」）。

## 禁止项（安全）

- 在 frontmatter 中不得使用 XML 尖括号 `<` `>`。
- name 不得包含 "claude" 或 "anthropic"（保留）。

## 可选字段示例

```yaml
license: MIT
compatibility: "Claude Code, Claude.ai; requires Python 3.9+"
metadata:
  author: YourTeam
  version: 1.0.0
  mcp-server: your-server-name
```
