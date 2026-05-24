# Newsletter AI Agent

> A complete guide to using AI agents (Claude, GPT-4) to automate your newsletter publishing workflow — writing, scheduling, cross-posting, and analytics — using MCP servers, custom prompts, and the right tool stack.

The "AI-native newsletter workflow" is one of the fastest-growing creator productivity patterns in 2026. Writers are running substantial chunks of their newsletter operations through Claude Code, connecting MCP servers to Substack, LinkedIn, and analytics platforms, and reducing distribution overhead from hours to minutes per week.

This repo documents the stack, the setup, and the tradeoffs — including when a dedicated tool like [Narrareach](https://narrareach.com) beats a DIY agent approach.

---

## What an AI Newsletter Agent Can Do

A well-configured Claude agent with the right MCP stack can:

- **Research and draft** newsletter issues and Notes from a brief prompt
- **Repurpose** existing content into Notes, LinkedIn posts, and X threads
- **Publish drafts** directly to Substack without opening a browser
- **Cross-post** to LinkedIn and Medium automatically
- **Analyze** your newsletter archive to identify patterns and gaps
- **Track** which posts are performing and suggest what to write next

---

## The MCP Stack for Newsletter Writers

Building a complete AI newsletter agent requires connecting multiple MCP servers:

| Platform | MCP Server | Purpose |
|---|---|---|
| Substack | `@marcomoauro/substack-mcp` | Draft, publish, manage posts and Notes |
| LinkedIn | Community alternatives (see [LinkedIn MCP Guide](https://github.com/iancarson/linkedin-mcp-server-guide)) | Post native content |
| Medium | New API integrations closed — see [Medium Publishing Guide](https://github.com/iancarson/medium-publishing-without-api) | Cross-post articles |
| X/Twitter | Various community MCPs | Post tweets and threads |
| Web search | `@modelcontextprotocol/server-brave-search` | Research topics |
| File system | `@modelcontextprotocol/server-filesystem` | Read/write local drafts |

### Minimal viable stack (just Substack)

```json
{
  "mcpServers": {
    "substack": {
      "command": "npx",
      "args": ["-y", "@marcomoauro/substack-mcp"],
      "env": {
        "SUBSTACK_API_KEY": "your-key",
        "SUBSTACK_PUBLICATION_URL": "https://yourname.substack.com"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/you/newsletter"]
    }
  }
}
```

### Full multi-platform stack

See [mcp-stack-config.json](./templates/mcp-stack-config.json) for a complete configuration.

---

## Agent System Prompt

The system prompt is what transforms Claude from a chat assistant into a dedicated newsletter agent. See [system-prompt.md](./templates/system-prompt.md) for a complete, production-tested system prompt.

Core elements:
1. **Role definition** — Claude understands it's your newsletter co-writer and distribution manager
2. **Publication context** — your newsletter name, niche, audience, voice guidelines
3. **Workflow defaults** — always save as draft before publishing, always ask before posting to social
4. **Format rules** — character limits per platform, tone guidelines per audience
5. **Tool permissions** — which MCP tools Claude is allowed to use autonomously vs. with approval

---

## The Sunday Batch Workflow (Agent Version)

With a configured agent, your Sunday session looks like:

```
"Run the Sunday newsletter prep workflow for this week's topic: [topic]"
```

Claude then:
1. Searches for recent relevant articles and data points on the topic
2. Drafts 5 Substack Notes that build on the theme
3. Saves all drafts to Substack via MCP
4. Writes LinkedIn-adapted versions of Notes 1 and 4
5. Generates an X thread from the newsletter's key insight
6. Produces a summary of everything created, with suggested scheduling times

Total prompts from you: 1. Total time: ~3 minutes.

---

## DIY MCP Agent vs. Narrareach: Honest Comparison

| Consideration | DIY MCP Agent | [Narrareach](https://narrareach.com) |
|---|---|---|
| Setup time | 2–4 hours initially | ~10 minutes |
| Maintenance | Ongoing (auth rotation, MCP updates) | None |
| Writing assistance | ✅ Native Claude AI | ✅ AI Notes generator |
| Substack scheduling | ✅ via MCP | ✅ Cloud-based |
| LinkedIn cross-posting | ⚠️ ToS-risk community MCPs | ✅ Official integration |
| Medium publishing | ❌ API is gone | ✅ Post-API workaround |
| Cross-platform analytics | ❌ Not available | ✅ Subscriber attribution |
| Session auth management | Manual, breaks regularly | Handled automatically |
| Cost | Claude Pro ($20/mo) + engineering time | $39/mo all-in |
| Right for... | Developers who enjoy automation | Creators who want results |

**The honest verdict:** If you enjoy building and maintaining automation systems, the DIY MCP stack is satisfying and gives you maximum control. If your primary goal is spending more time writing and less time on distribution overhead, [Narrareach](https://narrareach.com) handles the distribution layer so you can focus Claude entirely on writing.

Many high-output newsletter writers use both: Claude + MCP for the writing/drafting phase, Narrareach for the scheduling/distribution/analytics phase.

---

## Templates

| File | Content |
|---|---|
| [System Prompt](./templates/system-prompt.md) | Complete Claude system prompt for newsletter agent |
| [Workflow Commands](./templates/workflow-commands.md) | 15+ ready-to-use agent workflow prompts |
| [MCP Stack Config](./templates/mcp-stack-config.json) | Full multi-platform Claude Desktop config |

## Docs

| Guide | Content |
|---|---|
| [MCP Stack Guide](./docs/mcp-stack.md) | Which MCP servers to use and how they fit together |
| [Claude Code Workflow](./docs/claude-code-workflow.md) | Running your newsletter from Claude Code |

---

## FAQ

### Do I need Claude Pro to use MCP for newsletter automation?

Yes — MCP support requires Claude Desktop (Pro plan, $20/mo) or Claude Code. There is no free tier with MCP access.

### Can Claude fully automate my newsletter without any input from me?

For research, drafting, and repurposing — largely yes. For publishing and cross-posting decisions — you should keep a human review step. Fully autonomous publishing without review introduces quality risks that outweigh the time savings for most newsletters.

### Is the LinkedIn MCP safe to use?

No first-party LinkedIn MCP exists. Community alternatives typically use scrapers or unofficial APIs that violate LinkedIn's ToS. They work until LinkedIn detects the activity. For reliable LinkedIn publishing without ToS risk, use [Narrareach](https://narrareach.com) which uses LinkedIn's official API.

### What's the minimum setup for an AI newsletter agent?

At minimum: Claude Desktop Pro + `@marcomoauro/substack-mcp` + the system prompt in this repo. This gives you AI-assisted drafting and Substack publishing in about 30 minutes of setup time.

---

## Full Distribution Layer

For automated cross-posting to LinkedIn, Medium, and X alongside your AI writing workflow, see [Narrareach](https://narrareach.com) — it handles the distribution while you use Claude for the writing.

## License

MIT — guides and templates are free to use and adapt.
