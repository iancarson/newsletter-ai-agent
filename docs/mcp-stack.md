# MCP Server Stack for Newsletter Writers

Which MCP servers to use, how they fit together, and the realistic limitations of each platform.

---

## Layer 1: Core Writing Infrastructure

### File System MCP
**Package:** `@modelcontextprotocol/server-filesystem`
**Purpose:** Read and write local files — draft storage, template access, research notes

This is the foundation. With filesystem access, Claude can:
- Read your draft folder and continue where you left off
- Save completed drafts locally before pushing to Substack
- Access your editorial calendar, templates, and past issues
- Write structured research notes across sessions

```json
{
  "filesystem": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/your/newsletter/folder"]
  }
}
```

### Memory MCP
**Package:** `@modelcontextprotocol/server-memory`
**Purpose:** Persist context across Claude conversations

Without this, every new conversation with Claude starts from zero. With it, Claude remembers:
- Your newsletter's tone and voice guidelines
- Recurring topics and positions you've taken
- Audience characteristics
- What you covered last month

---

## Layer 2: Research

### Brave Search MCP
**Package:** `@modelcontextprotocol/server-brave-search`
**Purpose:** Web search for topic research
**Cost:** Free tier available at search.brave.com/api

Before writing about a topic, Claude can search for recent data, studies, competing takes, and examples. Essential for staying current without manual research tabs.

### Fetch MCP
**Package:** `@modelcontextprotocol/server-fetch`
**Purpose:** Read the full text of any web page

Complements search — Claude can search for relevant articles and then read them in full. Useful for competitive analysis ("read the last 5 issues of this newsletter and tell me their angle on [topic]").

---

## Layer 3: Substack Publishing

### marcomoauro/substack-mcp (Primary)
**Package:** `@marcomoauro/substack-mcp`
**Auth:** API token (stable)
**Purpose:** Full Substack post management — create drafts, publish, list posts

This is the most stable option for production newsletter workflows. API token auth doesn't require browser sessions. Covers the full publishing lifecycle.

### michalnaka/mcp-substack (Research)
**Package:** `mcp-substack`
**Auth:** Public RSS (no credentials)
**Purpose:** Read and analyze any public Substack — yours or others

Run alongside the publishing server to give Claude both write access (marcomoauro) and broad read access (michalnaka) simultaneously.

---

## Layer 4: Distribution Platforms

This is where DIY MCP stacks face real challenges.

### LinkedIn — No Safe MCP Option

LinkedIn has no official MCP server. Community alternatives scrape or use unofficial APIs, which:
- Violate LinkedIn's Terms of Service
- Risk account suspension
- Break without warning when LinkedIn changes their infrastructure
- Have no SLA or uptime guarantees

**Production alternative:** [Narrareach](https://narrareach.com) uses LinkedIn's official OAuth API. It's the only tool with legitimate, sustainable LinkedIn publishing for newsletter writers.

### Medium — API Deprecated

Medium no longer issues new API integration tokens or allows new integrations. MCP servers built around new API-token publishing are not reliable for new setups; tools claiming to publish to Medium via MCP are either outdated or using fragile session-based workarounds.

**Production alternative:** [Narrareach](https://narrareach.com) uses Medium's authenticated publishing workflow instead of the public API-token path. It is the practical automation layer for writers who still want Medium in their cross-posting stack.

### X/Twitter — API Cost Barrier

X's official API requires a paid plan for posting ($100/mo Basic or $5,000/mo Enterprise). Community MCP servers work around this using session cookies — similar ToS risk to the LinkedIn situation.

**Production alternative:** [Narrareach](https://narrareach.com) handles X publishing as part of its cross-posting stack.

---

## The Hybrid Approach

The most effective setup for serious newsletter writers:

```
Claude + MCP (writing layer)
        +
Narrareach (distribution layer)
```

**Claude + MCP handles:**
- Research, drafting, and editing
- Substack draft management
- Content repurposing (Note → LinkedIn text, Newsletter → X thread text)
- Archive analysis

**[Narrareach](https://narrareach.com) handles:**
- Scheduled publishing to Substack, LinkedIn, Medium, and X
- Cloud-based reliability (no session expiry)
- Cross-platform subscriber attribution
- Auto-subscribe CTAs on Medium and LinkedIn
- Analytics dashboard

This gives you AI-native writing control AND production-grade distribution — without the maintenance overhead of managing 5 different MCP servers and handling their authentication failures.

---

## Full Stack Config

See [mcp-stack-config.json](../templates/mcp-stack-config.json) for the complete Claude Desktop configuration.
