---
name: developing-content
description: Researches high-performing social content into a Notion Idea, then turns the creator's Rough Draft into an Edited Draft while preserving their voice. Use for topic discovery, social post and transcript research, organizing a stream-of-consciousness draft, or suggesting additions, hooks, titles, visuals, and media.
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
      - API-patch-page
      - API-update-page-markdown
      - API-query-data-source
      - API-retrieve-a-data-source
      - API-create-a-comment
      - API-retrieve-a-comment
---

# Developing Content

Run a focused, Idea-based content workflow in Notion:

1. research high-performing social content into `Initial Research`;
2. wait for the creator to add their stream of consciousness to `Rough Draft`; and
3. create `Edited Draft` in the creator's words, with clearly separated agent suggestions.

Do not create projects, inboxes, outlines, reviews, or separate visual pages.

## Workspace

Use these stable Notion IDs:

- Content Factory root: `3c1bebc7-23c3-815e-b7d6-ea7518b07858`
- Ideas database: `3c1bebc7-23c3-81ef-bf32-f6cf57b95833`
- Ideas data source: `3c1bebc7-23c3-8196-9bda-000bd5f9a53d`

Notion is the source of truth. Do not create a parallel repository copy.

Ideas use `Name`, `Status`, `Topic`, `Profile`, and `Created`. Use only `New`, `Selected`,
`Parked`, and `Discarded`.

Each Idea owns these child pages:

- `Initial Research` — agent-generated source research
- `Rough Draft` — creator-generated stream of consciousness or transcript
- `Edited Draft` — agent-edited version of Rough Draft

Keep requested visual or media URLs directly on the Idea page under `## Visuals and media`.

## Audience and ownership

The primary audience is business owners and team leaders over 40 at medium-to-large businesses.
Favor useful specificity about implementing AI in real organizations over generic reach. When
relevant, include adoption, permissions, privacy, security, compliance, governance, integration,
reliability, ownership, measurement, and scaling beyond a pilot.

The creator owns idea selection, angle, thesis, opinions, stories, examples, final wording, and
publication decisions. The agent owns source research, organization, grammar cleanup, and optional
suggestions. Never silently turn an agent inference or source claim into the creator's opinion.

## Start every request

1. Determine whether the creator supplied a topic, requested topic suggestions, selected an Idea,
   or asked to edit a Rough Draft.
2. Query Ideas and read the relevant Idea, child pages, and comments. Do not ask for information
   already in Notion.
3. If several Ideas match, ask one focused question listing the choices.
4. Perform only the requested phase. Do not silently proceed from research into drafting.

## Research a topic or discover one

1. Inspect available source-specific social search and scraping skills before using generic web
   search. Search platforms where titles or opening text, engagement, and transcripts are available.
2. Find a small set of representative posts with visible evidence of performance. Record platform,
   creator, URL, publication date, collection date, and visible views, likes, comments, shares, or
   other metrics. If performance cannot be observed, label it `performance unverified`; do not call
   it high-performing. Treat raw metrics cautiously across accounts and platforms.
3. Read or transcribe enough of each source to understand its actual content, not just its title.
   Verify quotations and distinguish source claims, established facts, and agent inference.
4. Analyze each useful source:
   - exact title or post opening
   - hook mechanism and promise
   - angle or point of view
   - concise transcript/content summary with main argument, examples, and payoff
   - visible performance and relevance
5. Synthesize repeated hooks, common angles, audience questions, contradictions, gaps, and places
   where the creator can add original experience.
6. Return the strongest candidates with possible titles, hooks, and angles. Link every source.
   Treat titles and hooks as inspiration, not text to copy.
7. Deduplicate against existing Ideas. Create or update an Idea with `Status: New` and store the full
   source roundup and synthesis in its `Initial Research` child page. Keep the Idea page itself clear
   except for child pages and visual or media links.

Do not select an Idea unless the creator asks.

## Prepare a selected Idea

1. Mark the Idea `Selected`.
2. Ensure `Initial Research` contains the completed research.
3. Ensure `Rough Draft` exists with only a short instruction for the creator to paste a
   stream-of-consciousness transcript, use a Notion transcription block, or attach accessible audio.
4. Stop at the creator writing gate. Never write Rough Draft for the creator.

## Create Edited Draft

1. Read the Idea, comments, `Initial Research`, `Rough Draft`, and existing `Edited Draft`.
2. Accept creator-written text, a Notion transcription block, or audio that available tools can
   access and transcribe. If Rough Draft is not substantive or audio is inaccessible, stop and say
   what the creator needs to add. Do not invent a draft from research alone.
3. Treat `Rough Draft` as immutable creator source material. Never clean, clear, move, or replace it.
4. Identify the creator's thesis, opinions, stories, examples, intended audience, and natural turns
   of phrase. Correct obvious transcription errors only when context makes the correction clear.
5. Reorder the material into a coherent progression. Remove verbal filler and accidental repetition;
   clean grammar, punctuation, and transitions. Preserve meaning, uncertainty, characteristic
   wording, and first-person voice. Mark unresolved ambiguity with a concise bracketed note.
6. Create `Edited Draft` under the same Idea. Its first section contains only the cleaned and
   organized creator material.
7. Add `## Agent Suggestions` after the draft. Keep every agent-originated addition there:
   - optional additions grounded in Initial Research
   - where each addition could fit and what it contributes
   - source links and verification caveats
   - possible titles and opening hooks aligned with the actual draft
   - overpromising or click-promise risks
8. Prefer a small set of high-value suggestions. Do not turn this into an outline or generic review.

If `Edited Draft` already contains substantive creator edits, do not replace it without explicit
permission. Offer a new version or targeted update.

## Visuals and media

When the creator requests visual or media support, add links directly under `## Visuals and media`
on the Idea page. Do not create a `Visuals` child page. Link only relevant assets or references and
briefly state what each supports.

## Finish every action

- Give useful research or editing outcomes in the response, not only a Notion link.
- State which Idea and child pages were created or updated.
- State unverified performance, transcript gaps, factual questions, and creator decisions.
- Do not publish or alter Rough Draft.
