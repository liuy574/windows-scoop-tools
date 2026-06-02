# Windows Scoop Tools

Windows Scoop Tools is a Codex skill for running commands reliably in Windows PowerShell when Unix-style command-line tools are installed through Scoop.

## Why This Skill Matters

AI coding agents often need to search files, inspect structured data, download assets, archive outputs, and run GitHub workflows from a shell. On Windows, those routine tasks can become inconsistent because PowerShell has aliases that shadow familiar command names, object pipelines behave differently from text pipelines, quoting rules are easy to get wrong, and tools such as `find`, `sort`, and `curl` can resolve to different executables than expected.

This skill gives the agent a small, opinionated command policy:

- Treat PowerShell as a command launcher, not the main data-processing language.
- Prefer predictable Scoop-provided CLI tools for common text, JSON, YAML, archive, diff, and GitHub tasks.
- Use native PowerShell cmdlets only where they are safer, especially for copy, move, and delete operations.
- Move complex loops, grouping, aggregation, and quoting-sensitive logic into Node `.mjs` scripts.

The result is less shell-specific improvisation and fewer failures caused by aliases, quoting, locale, executable-name collisions, or fragile one-liners.

## What It Covers

The skill defines preferred commands for:

- Text search with `rg`
- File discovery with `fd`
- File reading with `bat`
- Directory listing with `eza`
- JSON and YAML inspection with `jq` and `yq`
- Text replacement with `sd`
- Archives with `7z`
- Downloads and network calls with `curl.exe`
- GitHub workflows with `gh`
- Diff display with `delta`
- Complex data processing with Node `.mjs` scripts

It also documents rules for avoiding PowerShell alias ambiguity, using `-LiteralPath` for filesystem mutations, avoiding brittle pipelines, and reporting missing tools instead of silently falling back to fragile alternatives.

## Installation

Place the folder in your Codex skills directory:

```powershell
$skills = Join-Path $env:USERPROFILE '.codex\skills'
New-Item -ItemType Directory -Force -Path $skills
Copy-Item -Recurse -LiteralPath '.\windows-scoop-tools' -Destination $skills
```

After installation, Codex can discover the skill from its `SKILL.md` metadata.

## Recommended Tools

Install the tools you want the skill to prefer through Scoop:

```powershell
scoop install ripgrep fd bat eza jq yq sd 7zip gh delta nodejs
```

The skill does not tell the agent to install tools automatically. If a preferred tool is missing, the expected behavior is to report that fact and choose the safest available path.

## Repository Contents

```text
windows-scoop-tools/
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
|-- README.md
`-- LICENSE
```

## License

MIT
