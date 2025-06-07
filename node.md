# Helix JavaScript/TypeScript Setup

## Installation

```bash
npm install -g typescript typescript-language-server
npm install -g prettier
```

## Configuration

Update `~/.config/helix/languages.toml`:

```toml
[[language]]
name = "javascript"
language-servers = ["typescript-language-server"]
formatter = { command = "prettier", args = ["--stdin-filepath", "file.js"] }

[[language]]
name = "typescript"
language-servers = ["typescript-language-server"]
formatter = { command = "prettier", args = ["--stdin-filepath", "file.ts"] }

[language-server.typescript-language-server]
command = "typescript-language-server"
args = ["--stdio"]
```
