# Haravan public marketplace

This repository is the public installer/catalog for the Haravan MCP plugin.
It intentionally contains only marketplace metadata, the plugin manifest, the
remote MCP endpoint, and workflow skills.

The API implementation is kept in a separate private repository and runs
server-side at `https://plugin-enc0.onrender.com/mcp`. Credentials and shop
tokens are never included in this repository.

## Install from Codex CLI

```bash
codex plugin marketplace add https://github.com/worktinhbuithanh-code/haravan-marketplace.git --ref main --sparse .agents/plugins
codex plugin marketplace list
```

Then install `Haravan SKU` from the marketplace and complete MCP OAuth when
the client asks for authorization.

## Contents

- `.agents/plugins/marketplace.json` — public marketplace catalog.
- `plugins/haravan-mcp/plugin.json` — portable plugin manifest.
- `plugins/haravan-mcp/mcp.json` — remote MCP server connection metadata.
- `plugins/haravan-mcp/skills/` — public workflow instructions.

This repository must not receive `.env` files, access tokens, API source code,
database dumps, deployment credentials, or private backend configuration.
