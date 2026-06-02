---
name: windows-scoop-tools
description: Use when running commands in Windows PowerShell where Scoop/Unix-style tools, aliases, quoting, locale, JSON/YAML, archives, GitHub CLI, or pipelines affect reliability.
---

# Windows Scoop Tools

## Purpose

Make command execution on Windows predictable for AI agents. PowerShell aliases, object pipelines, quoting rules, locale behavior, and executable-name collisions can make ordinary shell work unreliable. This skill narrows command choices to stable Scoop-style tools and moves fragile data processing into scripts.

## Core Rule

Use PowerShell as a command launcher. Use the single default command below for common work. Move complex data processing into Node `.mjs` scripts.

## Defaults

| Task | Command |
| --- | --- |
| Search text | `rg` |
| Find files | `fd` |
| Read file | `bat --style=plain --paging=never` |
| Read line range | `bat --style=plain --paging=never --line-range A:B` |
| List directories | `eza` |
| JSON inspect | `jq` |
| YAML inspect | `yq` |
| Replace text | `sd` |
| Archive | `7z` |
| Network | `curl.exe` |
| GitHub | `gh` |
| Diff display | `delta` |
| Batch or structured processing | Node `.mjs` script |

## Rules

1. Use direct commands only for simple search, find, list, read, inspect, archive, download, and diff actions.

2. Use a Node `.mjs` script for loops, grouping, aggregation, object pipelines, complex regex/quoting, string interpolation, multi-step JSON/YAML transformation, nested commands, or more than one pipe. Create temporary scripts only when file writes are allowed.

3. For copy, move, and delete, use PowerShell native cmdlets with `-LiteralPath`. Do not use `cp`, `mv`, `rm`, `del`, or other aliases.

4. When executable behavior matters, call the real executable:

```powershell
find.exe . -name "*.ts"
sort.exe input.txt
curl.exe https://example.com
```

5. Quote search patterns with single quotes by default. Use `rg -F` for fixed strings. If quoting becomes complex, move the logic to Node.

```powershell
rg -n 'defs\.put\("|aggregate|between|like' .
rg -F 'literal text with "quotes"' .
```

6. Do not set PATH unless a preferred Scoop command is missing. If needed, prepend only Scoop shims:

```powershell
$env:PATH = "$env:USERPROFILE\scoop\shims;$env:PATH"
```

7. Do not install tools unless the user asks. If a preferred tool is unavailable, report it instead of inventing a complex PowerShell replacement.

8. Avoid `sed`, `grep`, and `awk` unless the user explicitly asks for them. If they are required and locale fails, set locale only for that command:

```powershell
$env:LANG='C'; $env:LC_ALL='C'; sed 's/a/A/' file.txt
```

## Avoid

- Long `pwsh -Command` one-liners.
- PowerShell object pipelines for data processing.
- `bat | sed` for line selection; use `bat --line-range A:B`.
- Styled or paged output for agent reads.
- Alias-dependent commands when executable behavior matters.
