<img src="assets/logo.png" alt="Moderation API" width="96" height="96">

# Moderation API MCP Server

Let AI agents run your [Moderation API](https://moderationapi.com) account: work through review queues, investigate authors, tune policies and wordlists, respond to incidents and report on how moderation is going.

This is a hosted (remote) [Model Context Protocol](https://modelcontextprotocol.io) server. Connect Claude, Cursor or any other MCP client to:

```
https://mcp.moderationapi.com/mcp
```

There is nothing to install. The server has one tool for each endpoint of the [REST API](https://docs.moderationapi.com/api-reference/introduction), and it calls the same API as your code, so rate limits, quotas and request logs work the same way.

## What your agent can do

- **Triage the review queue.** "Go through today's unresolved items, reject the obvious spam, and show me the 5 you're unsure about." The agent does the first pass; you make the judgment calls.
- **Investigate an author.** "Is user 8812 a bad actor? Check their history and suspend them if it's a pattern."
- **Tune policies from real examples.** "These 10 comments got flagged but are fine, and these 3 got through. Fix it." The agent tests the examples without storing them, finds the thresholds or wordlist entries that separate them, and updates the channel.
- **Respond to incidents.** "There's a raid of crypto scam posts in the last hour. Find them, block the authors, and add the domains to the blocklist."
- **Set up moderation without the dashboard.** "Set up moderation for our new marketplace: a channel for listings, one for messages, and a webhook to our endpoint." Then send test content to check it.
- **Report and audit.** "How did moderation go this month?" Queue stats, actions and author trends turned into a summary.
- **Combine with your other tools.** A daily queue digest in Slack, an issue when a policy keeps misfiring, a draft reply to the user whose post was removed.

The server also ships prompts (recipes) for these jobs: `triage-queue`, `check-author`, `tune-policy`, `incident-response` and `moderation-report`. More example prompts: [What your agent can do](https://docs.moderationapi.com/ai/examples).

## Tools

| Area | What the tools do |
| - | - |
| Content | Submit content and read the result, with or without storing it; allow or reject content |
| Review queues | List queues and items, resolve and unresolve items one by one or up to 100 at a time, queue statistics |
| Authors | List, look up, create and update authors; block, suspend and unblock them |
| Actions | List and run moderation actions on content and authors; create and change actions |
| Policies | Read and change channels and their policies; create and change wordlists and their words |
| Analytics | Summary, most flagged policies and authors, numbers per channel, volumes over time, automation |
| Projects | List, create, change and duplicate projects; create API keys |
| Webhooks | List, create and change webhooks |
| Feedback | Send a feature request to the Moderation API team |

Read-only tools are marked as read-only, so your agent can run them without asking. Tools that change or delete data are marked as such, and most agents ask you before they run them.

## Setup

You need a Moderation API account with the **Owner**, **Admin** or **Developer** role. [Sign up free](https://moderationapi.com).

### Claude Code

```bash
claude mcp add --transport http moderation-api https://mcp.moderationapi.com/mcp
```

Or install the [plugin](https://docs.moderationapi.com/ai/overview), which adds the MCP server and the setup skill together.

### Claude and Claude Desktop

Open **Settings → Connectors → Add custom connector**, and enter `https://mcp.moderationapi.com/mcp`.

### Cursor

Add the server to `.cursor/mcp.json` in your project, or to `~/.cursor/mcp.json` for all projects:

```json
{
  "mcpServers": {
    "moderation-api": {
      "url": "https://mcp.moderationapi.com/mcp"
    }
  }
}
```

### Other clients

Add a remote MCP server with the URL `https://mcp.moderationapi.com/mcp` and the Streamable HTTP transport.

## Sign in

The first time your agent uses the server, your browser opens:

1. Sign in with your Moderation API account.
2. Pick the organization the agent can use.
3. Approve the connection.

The agent can reach the projects that you can reach. Each connection gets its own API key, shown on the **API keys** page and named after the app and your email. Delete the key to end the connection. When the agent resolves queue items or runs actions, the review queue shows you as the reviewer.

### Without a browser

Agents that cannot open a browser, for example in CI, can send an API key instead:

```bash
claude mcp add --transport http moderation-api https://mcp.moderationapi.com/mcp \
  --header "Authorization: Bearer $MODAPI_SECRET_KEY"
```

A project key reaches one project. An organization key reaches the projects you allow it to.

## Projects

Tools that work on content in a project take an optional `projectId`. You can give the project ID or its name. When it's left out and the key reaches more than one project, the tool returns the projects to choose from.

## Other tools for agents

- **Setup skill**: lets a coding agent add Moderation API to your app's code. `npx skills add moderation-api/skills`
- **CLI**: sign in, create a project and write API keys from the terminal. `modapi init`

See [Build with AI agents](https://docs.moderationapi.com/ai/overview).

## Links

- [Documentation](https://docs.moderationapi.com/ai/mcp)
- [Moderation API](https://moderationapi.com)
- [MCP Registry listing](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.moderationapi): `com.moderationapi/moderation-api`

## License

MIT
