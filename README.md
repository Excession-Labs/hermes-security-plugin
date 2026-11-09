# hermes-security-plugin

A [Hermes Agent](https://github.com/NousResearch/hermes-agent) plugin that adds two remote MCP servers and one skill:

- `pts` is the MCP server of [penetrationtestservices.com](https://penetrationtestservices.com). Its `check_security_headers` tool checks the security headers of a public URL. It also has `search_site`, `get_page`, `how_to_contact` and `request_pentest`.
- `pc` is the MCP server of [pentestcompare.com](https://pentestcompare.com). Its `list_providers` and `get_provider` tools return sourced data on penetration testing providers.
- The skill `check-security-headers` tells the agent when to use which tool.

Both sites belong to [Excession Labs](https://excessionlabs.ai), a security lab.

This is a portable Agent Plugins v1 package: `plugin.json`, `mcp.json` and `skills/`. It has no Python code.

## Install

```bash
hermes plugins install Excession-Labs/hermes-security-plugin --no-enable
hermes plugins enable hermes-security-plugin
```

Start a new session. No API key is needed for header checks or provider data.

## Disclosure

- Network calls: the tools call `https://penetrationtestservices.com/mcp` and `https://pentestcompare.com/mcp`. A header check sends the URL you give to penetrationtestservices.com, which then fetches that URL with one GET request per redirect.
- `request_pentest` sends a contact request (name, email, targets) to Excession Labs. It needs an agent key from `POST https://penetrationtestservices.com/agent/auth`, and the plugin does not hold one. Both servers are marked `trust: untrusted`, so Hermes asks before each call that is not read-only.
- The plugin reads no files outside its own directory, runs no shell command and starts no background process. It stores no credential and sends no telemetry.
- It does not update itself.

## Check it

```bash
hermes plugins validate /path/to/hermes-security-plugin
```

## Limits

A header check reads one HTTP response. It does not show that a site is secure. The service allows 12 checks per minute per IP address.

## License

MIT
