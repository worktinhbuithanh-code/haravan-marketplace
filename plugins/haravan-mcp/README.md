# Haravan MCP plugin

This package connects a supported plugin host to the existing Haravan MCP
server at `https://plugin-enc0.onrender.com/mcp`. It contains only the public
plugin manifest, connection metadata, and workflow skills. It does not contain
the backend API implementation, shop credentials, Haravan access tokens, or
knowledge-admin credentials.

Authentication is performed by the server's configured MCP OAuth flow; shop
tokens remain server-side.
