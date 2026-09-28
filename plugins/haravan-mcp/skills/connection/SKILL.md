---
name: connection
description: Check Haravan MCP connection or authorization status and explain the next safe diagnostic step.
---

Use this skill for connection, authorization, installation, or availability
questions. Call `haravan_connection_status` and report its actual result. Use
`haravan_profile` when the user needs to know which authenticated Haravan
profile is connected. Do not discover or execute a business API request merely
to check whether the connection works.

If status reports a problem, explain the failure at the level exposed by the
tool and suggest the smallest next step. Never request, display, or transmit
access tokens, authorization headers, shop selectors, or other credentials.
