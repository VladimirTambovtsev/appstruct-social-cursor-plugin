# AppStruct Social for Cursor and Claude Code

Connect Cursor or Claude Code to AppStruct Social to inspect connected channels, read posts and measured analytics, and save **unscheduled drafts** for review.

This repository distributes client configuration and a workflow skill. The hosted service runs at `https://api.appstruct.ai/v1/social/mcp`; its server implementation is maintained separately. MCP does not schedule or publish posts.

## Start with one draft in Cursor

1. Sign in to AppStruct Social, choose the intended workspace and connect the channel you want to use.
2. Open [Social API connection settings](https://appstruct.ai/dashboard/social/api/connect). Create a short-lived key with `social:read` and, to save drafts, `social:drafts:write`. Add `social:analytics:read` only when you need metrics. Check your account's current Social access and plan limits.
3. Install this plugin from an available marketplace listing, or follow Cursor's [local plugin development instructions](https://cursor.com/docs/plugins) with this repository. A repository release does not itself make the plugin available in a marketplace.
4. Configure the plugin variable `APPSTRUCT_SOCIAL_MCP_KEY` in your client's private plugin configuration. Keep the actual key out of this repository. Plugin variables use `${APPSTRUCT_SOCIAL_MCP_KEY}`; the separate manual connection file below uses the client's `${env:APPSTRUCT_SOCIAL_MCP_KEY}` syntax.
5. Refresh tool discovery and try: “List my AppStruct Social channels. Show the platform and channel ID. Prepare a short fictional product-update draft for the destination I select, and return its review link. Do not schedule or publish it.”

Inspect the saved text and destination in Social before selecting a publishing time. If you use a team-managed plugin, check who can use its shared connection before configuring a workspace key.

The [worked guide and practice brief](https://blog.appstruct.ai/blog/social-media-mcp-server?utm_source=github_plugin&utm_medium=referral&utm_campaign=social_mcp_launch_20261114) explain the full read, draft and review sequence. You can also use the [manual Cursor configuration](https://blog.appstruct.ai/downloads/appstruct-social-cursor-mcp.json) without installing this plugin. Choose one connection method to avoid two copies of the same server.

## Connect Claude Code

Set `APPSTRUCT_SOCIAL_MCP_KEY` in the private environment that starts Claude Code. Use a key for the intended AppStruct workspace, with `social:read` and, to save drafts, `social:drafts:write`. Do not put a real key in a command copied into a shared terminal transcript or a committed file.

Run these commands in a terminal, from the project where you want to use the connection:

```sh
claude plugin marketplace add VladimirTambovtsev/appstruct-social-cursor-plugin
claude plugin install appstruct-social@appstruct-social-plugins --scope local
```

The repository name retains its original Cursor name; this release contains separate manifests for both clients. This is an AppStruct-maintained marketplace, not an Anthropic directory approval. The `local` scope enables the plugin for this project and user. Review the connection permissions Claude Code shows before enabling it, then open `/mcp` and confirm `appstruct-social` is connected. If another method already adds this server, use that connection instead of installing a second copy.

Start with: “Use AppStruct Social to list connected channels with their platform, ID and status. Do not save a draft yet.” Compare those destinations with your selected Social workspace. Then follow the [draft and read-back exercise](https://blog.appstruct.ai/blog/social-media-mcp-server?utm_source=claude_plugin&utm_medium=referral&utm_campaign=social_mcp_launch_20261114).

The plugin uses `.mcp.json` with an HTTP transport and `${APPSTRUCT_SOCIAL_MCP_KEY}` environment expansion. Cursor's manual `${env:APPSTRUCT_SOCIAL_MCP_KEY}` syntax is different. See the [Claude Code MCP reference](https://code.claude.com/docs/en/mcp) and [plugin marketplace instructions](https://code.claude.com/docs/en/plugin-marketplaces). Missing or expired credentials require reconnecting with the intended key; installing the package alone does not authenticate it.

## Included tools

| Task | Tool |
| --- | --- |
| Inspect connected channels | `social_list_channels` |
| Find a saved post | `social_list_posts` |
| Read full content and revision | `social_get_post` |
| Check a saved draft | `social_validate_draft` |
| Read available measured totals | `social_get_analytics` |
| Save an unscheduled draft | `social_create_draft` |
| Replace an unscheduled draft after reading it | `social_update_draft` |

Tools available to a connection depend on its scopes. The current [OpenAPI document](https://api.appstruct.ai/v1/social/openapi.json) lists the MCP tool names and required permissions. Clients restricted to OAuth sign-in cannot use this Bearer-key connection.

## Package contents

- `.cursor-plugin/plugin.json`: discovery metadata and the key-variable declaration.
- `mcp.json`: the remote server URL and a variable placeholder, with no credential.
- `.claude-plugin/plugin.json` and `marketplace.json`: Claude Code plugin and AppStruct marketplace metadata.
- `.mcp.json`: Claude Code's HTTP connection and environment placeholder.
- `skills/appstruct-social/SKILL.md`: resource selection, draft revision and retry guidance.
- `assets/appstruct.svg`: the public AppStruct logo.

There are no package dependencies, executable hooks or bundled server process. The package does not contain a key and does not grant access until a user configures a connection. The read scope includes unpublished posts and attachments in that workspace.

## Validation and availability

Version 1.1.0 adds Claude Code packaging and installation instructions. The manifests are validated locally, and the tool list matches the live Social OpenAPI document. The public MCP endpoint returns HTTP 401 without a key. An authenticated client draft run was not performed for this package release; use the first-read and draft checks above with your own intended workspace.

This is published by AppStruct. Marketplace review, search indexing and adoption are separate from the repository release. View the hosted server's [official MCP Registry record](https://registry.modelcontextprotocol.io/?q=io.github.VladimirTambovtsev%2Fappstruct-social), or [explore AppStruct Social](https://appstruct.ai/social?utm_source=github_plugin&utm_medium=referral&utm_campaign=social_mcp_launch_20261114&utm_content=repo_product).

## License

Client configuration and skill text are MIT licensed. The hosted AppStruct service has its own terms and plan limits. AppStruct's name and logo are retained brand assets; see `NOTICE`.
