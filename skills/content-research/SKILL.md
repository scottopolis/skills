---
name: content-research
description: Researches videos for content aimed at owners of $1M+ businesses and leaders at larger companies by analyzing hooks, actual content, and revealing audience comments. Use for content research, topic discovery, or finding evidence-backed audience angles.
license: MIT
mcpServers:
  notion:
    command: npx
    args: ["-y", "@notionhq/notion-mcp-server@2.5.1"]
    env:
      NOTION_TOKEN: "${NOTION_API_KEY}"
    includeTools:
      - API-post-search
      - API-retrieve-a-page
      - API-retrieve-page-markdown
      - API-post-page
      - API-query-data-source
      - API-update-page-markdown
---

# Content Research

Find what is working, understand why, and preserve the useful evidence in a short research page.
Research only; do not create drafts, visual concepts, or publication plans.

## Audience Fit

The primary audience is owners of businesses doing at least $1M in revenue and leaders at larger
companies. Do not target indie founders, solo builders, or developers unless the creator explicitly
asks to.

Source creators may primarily serve those audiences. Use their videos as signals for useful hooks,
questions, and ideas, not as an audience template. Translate relevant concepts into the decisions,
constraints, and outcomes that matter to established businesses. Optimize for reaching the right
audience, not for maximizing views; popularity is evidence that a source earned attention, not the
goal of the resulting content.

The creator may still choose a topic simply because it is interesting. Include those topics when
requested or unusually compelling, but label them `interest-led` and state that the audience fit is
weak. Do not present them as audience-growth strategy.

## Platform References

Read [reference/youtube.md](reference/youtube.md) before researching YouTube. It contains the
platform-specific discovery, transcript, comment, and provider workflow. Keep platform mechanics in
reference files so this skill's research process and deliverable remain consistent.

## Research

1. Clarify the topic and intended audience. Search existing Ideas before creating a duplicate.
2. Search relevant video platforms and the web. Riley Brown, Peter Yang, Simon Scrapes, and Nate
   Herk are useful starting points, not a closed list; follow the topic to other relevant creators.
3. Select a small set of highly relevant videos. Prioritize ideas that can serve the target audience;
   use visible views or engagement only as supporting evidence, in the context of the creator and
   publication date.
4. Open each source and inspect its transcript, captions, or video content. Never infer the content
   from the title alone. Record unavailable transcripts or metrics instead of guessing.
5. Read a small sample of relevant comments when available. Look for questions, objections,
   repeated language, surprising interpretations, and gaps that expose an interesting angle.
6. Use public pages and available search, browser, transcript, or API tools to collect this evidence.
   Scraping is a means of research, not a separate deliverable. Respect access restrictions and do
   not download videos, bypass controls, or claim comprehensive comment coverage.
7. Add a page to the `Ideas` board, then add the research as a subpage of that Idea. Do not rely on
   hard-coded workspace, database, or data-source IDs.

## Deliverable

Keep the research concise. For each selected video include:

- **Source:** linked video, creator, and only the performance context needed to explain its inclusion
- **Hook:** the opening words or move and why it earns attention
- **Content:** what the video actually says, including its main argument, examples, and payoff
- **Comment angle:** the most useful audience question, tension, objection, or insight, when available
- **Audience fit:** how the idea applies to the target audience, or `interest-led` when it does not

Finish with a short synthesis of repeated hook patterns, what appears to resonate, and promising
audience tensions or questions. Link every video and clearly label missing or unverified evidence.
