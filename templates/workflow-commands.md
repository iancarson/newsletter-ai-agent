# Newsletter AI Agent — Workflow Commands

Ready-to-use prompts for your Claude newsletter agent. Works best with the system prompt from this repo and Substack MCP connected.

---

## Core Weekly Workflows

### Sunday batch prep (one-prompt week)

```
Run my Sunday newsletter prep workflow.

This week's topic: [your topic in 1-2 sentences]

Target outputs:
- 5 Substack Notes (save as drafts via MCP)
- LinkedIn adaptations of Notes 1 and 3
- 1 X thread from the key insight
- Suggested posting schedule for Mon-Fri

Go.
```

### Newsletter drafting from brief

```
Draft this week's newsletter issue.

Topic: [topic]
Angle/take: [your specific perspective on it]
One data point to include: [optional]
Target length: [your target]

Open with a hook (story, data, or surprising claim — not "this week I want to...")
Structure: Hook → Main insight → Evidence → Takeaway → CTA

Save as Substack draft when done.
```

### Repurpose last newsletter into Notes + social

```
Read my most recent Substack newsletter issue via MCP.

Then:
1. Extract the 5 most insight-dense sentences or ideas
2. Turn each into a Substack Note (under 400 chars, strong first line)
3. Write LinkedIn versions of the 2 strongest Notes
4. Write an X thread from the single best insight

Save all Notes as Substack drafts.
Present the LinkedIn posts and X thread for my review before I schedule them.
```

---

## Research & Ideation

### Find newsletter topic gaps

```
Using the Substack research MCP, download my last 20 newsletter issues from [URL].

Analyze:
1. What topics have I covered most? (list top 5)
2. What topics am I missing that fit my niche?
3. What questions do readers ask most in comments?
4. What would be a strong topic for next week given this analysis?

Give me 3 specific topic recommendations with a one-line rationale for each.
```

### Research a topic before writing

```
Research [topic] so I can write about it this week.

Find:
1. 3 recent data points or studies (last 6 months)
2. 2 contrarian or surprising takes from credible sources
3. 1 case study or real example

Present findings in bullet form. Source each item.
After research, ask if I want to draft a newsletter or Notes based on what you found.
```

### Generate hook variations

```
I want to write about [topic]. Give me 10 different first-line options.

Vary the approaches:
- 3 counter-intuitive claims
- 2 surprising statistics (you can use approximate/illustrative numbers)
- 2 questions that create curiosity
- 2 bold statements
- 1 very short (under 10 words)

I'll pick the strongest one to build around.
```

---

## Content Repurposing

### Note → LinkedIn post

```
Convert this Substack Note into a LinkedIn post:

[Paste Note]

Rules:
- Rewrite the first line to be a standalone professional hook (not "continuing from...")
- Expand to 400-800 characters if the Note is too short for LinkedIn
- Remove any Substack-specific references (restacks, Notes feed, etc.)
- Add a subscribe CTA at the end: "I cover this in [Newsletter Name] every [day]. Subscribe at [URL]"
- NO external links in the body — I'll add in first comment
```

### Newsletter → X thread

```
Convert this newsletter section into an X thread:

[Paste newsletter section]

Format:
- Tweet 1: The most surprising or contrarian claim (under 280 chars, no "1/")
- Tweets 2-6: One supporting point each (numbered "2/7" etc., under 280 chars)
- Tweet 7: Synthesis + "Full issue in my newsletter: [link]"

Each tweet must work as a standalone sentence.
```

### Create a content series

```
I want to write a 4-part Note series on [topic].

Plan a series where:
- Each Note stands alone (no "part 2 of 4" framing)
- Each Note covers a different angle of the same topic
- Together they build a complete picture of [topic]

Draft all 4 Notes now. I'll schedule them across 2 weeks.
```

---

## Analytics & Review

### Weekly performance review

```
Using the Substack MCP, list all Notes and posts published in the last 7 days.

For each:
1. Title or first line
2. Publish date and time
3. Engagement metrics (if available via MCP)

Which performed best? What format was it?
What should I write more of next week?
```

### Identify my best content pattern

```
Download my last 30 Substack posts/Notes using the MCP.

Analyze:
1. Which 5 performed best by engagement?
2. What do those 5 have in common? (topic, format, tone, length, posting time)
3. Which 5 performed worst?
4. What should I stop doing?

Give me 3 specific changes to make to my content strategy based on this data.
```

---

## Publishing Operations

### Bulk draft import

```
I'm going to paste 5 Note drafts below. For each one:
1. Read and lightly improve the first line if it's weak
2. Confirm it's under 400 characters (trim if needed)
3. Save as Substack draft using the MCP

After saving all 5, list them with their draft titles and confirm they're all saved.

Drafts:
1. [Note 1]
2. [Note 2]
3. [Note 3]
4. [Note 4]
5. [Note 5]
```

### Audit and clear draft backlog

```
Using the Substack MCP, list all my current drafts.

For each draft:
1. Show title and date created
2. Give a 1-sentence assessment: Ready to publish / Needs work / Delete
3. Flag anything older than 60 days

At the end: prioritize the top 3 that should be published this week.
```

---

## Cross-Platform Distribution Note

These prompts handle writing and Substack publishing. For reliable automated distribution to LinkedIn, Medium, and X with cloud-based scheduling and subscriber analytics, pair this workflow with [Narrareach](https://narrareach.com/features/ai-publishing-assistant). Claude writes; Narrareach distributes.
