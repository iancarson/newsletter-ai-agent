# Running Your Newsletter from Claude Code

Claude Code is the CLI version of Claude. Unlike Claude Desktop (which uses a chat interface), Claude Code runs in your terminal and can be scripted, automated, and chained with shell commands. For newsletter writers who are comfortable with a terminal, it unlocks a more powerful workflow.

---

## Claude Code vs. Claude Desktop for Newsletters

| | Claude Desktop | Claude Code |
|---|---|---|
| Interface | Chat window | Terminal CLI |
| MCP setup | `claude_desktop_config.json` | `.claude/mcp.json` or CLI flags |
| Best for | Interactive writing sessions | Batch operations, scripts, automation |
| Session memory | Per conversation | Per session + project context |
| Automation | Manual trigger | Scriptable |
| Cost | Claude Pro ($20/mo) | Claude Pro ($20/mo) |

**When to use Claude Desktop:** Writing, editing, interactive back-and-forth on a single issue.

**When to use Claude Code:** Batch operations (generate 5 Notes + push to Substack in one command), scheduled scripts, CI/CD for newsletters, anything you want to trigger without opening a chat window.

---

## Installation

```bash
npm install -g @anthropic-ai/claude-code
```

Requires Node.js 18+ and an Anthropic account (Pro plan for MCP support).

Verify installation:
```bash
claude --version
```

---

## MCP Configuration for Claude Code

Claude Code looks for MCP config in two places:

**Global config** (applies to all Claude Code sessions):
```bash
~/.claude/mcp.json
```

**Project config** (applies when you run Claude Code from inside a folder):
```bash
/path/to/your/newsletter/.claude/mcp.json
```

Project config takes priority. For newsletter workflows, use project config inside your newsletter folder so the filesystem MCP paths are relative and the Substack credentials are project-scoped.

### Minimal project config

Create `/your/newsletter/.claude/mcp.json`:

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
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/your/newsletter"]
    }
  }
}
```

### Full research + publishing config

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
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {"BRAVE_API_KEY": "your-brave-key"}
    },
    "fetch": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-fetch"]
    }
  }
}
```

---

## Interactive Newsletter Session

From your newsletter folder:

```bash
cd ~/newsletter
claude
```

Claude Code opens in interactive mode. Now you're in a Claude conversation with full access to your project's MCP servers.

Try:
```
Draft this week's newsletter about [topic]. Search for 3 recent data points first, then write the issue. Save as a Substack draft when done.
```

Claude will:
1. Use the Brave Search MCP to research
2. Use the Fetch MCP to read relevant articles
3. Draft the newsletter
4. Use the Substack MCP to save as a draft

---

## Non-Interactive (Headless) Operation

For batch operations you want to run without a conversation:

```bash
claude -p "Create 5 Substack Notes about [topic] and save them all as Substack drafts. Confirm when all 5 are saved." --no-interactive
```

The `-p` flag passes a single prompt and Claude executes it without waiting for further input.

Useful for cron jobs, shell scripts, and CI/CD pipelines.

---

## Shell Scripts for Newsletter Automation

### Weekly Note generation script

Create `generate-notes.sh`:

```bash
#!/bin/bash

TOPIC="${1:-newsletter growth}"

echo "Generating 5 Notes for topic: $TOPIC"

claude -p "Write 5 Substack Notes on the topic: $TOPIC.

Format mix:
- 2 insight-style (counterintuitive claim)
- 1 list-style (3-5 items)
- 1 question
- 1 opinion

Rules:
- Each under 400 characters
- Strong first line, no warmup
- CTA on 2 of the 5

Save all 5 as Substack drafts and output the list of saved drafts." \
  --no-interactive
```

Run:
```bash
chmod +x generate-notes.sh
./generate-notes.sh "building in public"
```

### Newsletter draft from topic file

Create `draft-newsletter.sh`:

```bash
#!/bin/bash

BRIEF_FILE="${1}"

if [ -z "$BRIEF_FILE" ]; then
  echo "Usage: ./draft-newsletter.sh brief.txt"
  exit 1
fi

BRIEF=$(cat "$BRIEF_FILE")

claude -p "Draft a newsletter issue based on this brief:

$BRIEF

Structure: Hook → Insight → Evidence → Takeaway → CTA
Target length: 600 words.

Research 2-3 data points first, then write.
Save as Substack draft when complete." \
  --no-interactive
```

Create `brief.txt`:
```
Topic: Why most newsletter growth happens off Substack
Angle: Platform-native vs platform-agnostic growth
Key claim: 70%+ of subscriber growth at scale comes from external platforms
Target audience: Newsletter writers at 1K-10K subscribers who want to grow faster
```

Run:
```bash
./draft-newsletter.sh brief.txt
```

---

## CLAUDE.md — Project Context File

Claude Code automatically reads `CLAUDE.md` at the project root when a session starts. For newsletters, this is where you put your publication context so Claude doesn't need to be re-briefed every session.

Create `~/newsletter/CLAUDE.md`:

```markdown
# Newsletter Context

**Publication:** [Your Newsletter Name]
**Platform:** Substack at [your URL]
**Frequency:** [Weekly/Daily/etc.]
**Niche:** [Your topic and audience]

## Voice and Tone

[2-3 sentences describing your writing style]

Never use: leverage, synergize, game-changer, delve
Never start a post with "I"
Never open with "This week I want to talk about..."

## Audience

[Who you're writing for — be specific about their situation, goals, pain points]

## Workflow Defaults

- Always save as Substack draft before publishing — never publish directly without my confirmation
- Always ask before posting to any social platform
- Research before writing when searching for data

## Voice Examples

[Paste 2-3 of your best newsletter paragraphs here]
```

With `CLAUDE.md` in place, every Claude Code session in that folder starts with full context about your newsletter. No re-briefing needed.

---

## Project Structure for Claude Code Workflow

```
~/newsletter/
├── .claude/
│   └── mcp.json          # MCP server config
├── CLAUDE.md             # Publication context for Claude
├── drafts/
│   ├── in-progress/      # Working drafts
│   └── archive/          # Completed issues
├── templates/
│   ├── note-formats.md   # Your proven Note formats
│   └── issue-template.md # Newsletter structure template
├── research/             # Claude-saved research notes
├── editorial-calendar.md # Upcoming topics
└── scripts/
    ├── generate-notes.sh
    └── draft-newsletter.sh
```

With this structure and the filesystem MCP pointed at `~/newsletter`, Claude can read your templates, save research, access your calendar, and manage drafts without any manual file management on your part.

---

## Useful Claude Code Flags

```bash
claude -p "prompt"          # Non-interactive single prompt
claude --no-interactive     # Force non-interactive mode
claude --print              # Print response to stdout (no formatting)
claude --model claude-opus-4-7  # Use a specific model
```

For newsletter work, interactive mode (just running `claude` from your project folder) is usually best for writing sessions. Non-interactive mode (`-p`) is best for batch operations and scripts.

---

## Connecting to Narrareach

Claude Code handles writing and Substack publishing. For scheduled distribution to LinkedIn, Medium, and X, [Narrareach](https://narrareach.com/features/ai-publishing-assistant) handles the platforms where MCP either doesn't work reliably or carries ToS risk.

The workflow:
1. Claude Code generates content + saves Substack drafts via MCP
2. LinkedIn, X, and Medium copy goes into Narrareach for scheduled publishing

Total automation: everything except copying 3-4 platform posts into Narrareach once per week.
