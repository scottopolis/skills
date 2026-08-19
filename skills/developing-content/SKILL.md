---
name: developing-content
description: Runs a Notion-based video content factory for AI business content. Use for idea discovery, source-specific research and video outlines, draft review, explanatory visuals, continuing a video project, or deciding the next step.
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
      - API-move-page
---

# Developing Content

Run the video Content Factory entirely in Notion. It has four agent actions: **ideation**,
**develop outline**, **review**, and **visuals**. Human writing is the gate between outline and review.

## Workspace

Use these stable IDs instead of discovering duplicate databases by title:

- Content Factory root: `3c1bebc7-23c3-815e-b7d6-ea7518b07858`
- Ideas database: `3c1bebc7-23c3-81ef-bf32-f6cf57b95833`
- Ideas data source: `3c1bebc7-23c3-8196-9bda-000bd5f9a53d`
- Projects database: `3c1bebc7-23c3-81ec-80c3-f0f09419af14`
- Projects data source: `3c1bebc7-23c3-81ed-b3d3-000be85c4c0b`

Notion is the source of truth for ideas, video outlines, creator drafts, reviews, and visual plans.
Do not create or update a parallel repository copy.

## Audience and roles

The primary audience is business owners and team leaders over 40 at medium-to-large businesses.
Build credibility around implementing AI in real organizations, not maximum generic reach.

The creator owns idea selection, angle, thesis, opinions, final wording, and publication decisions.
The creator writes the `Video Draft`. The agent discovers ideas, researches concrete source material,
creates a potential `Video Outline`, reviews the creator's draft below it on the same page, and creates
explanatory visuals. Do not write or rewrite the creator's draft unless explicitly asked.

When relevant, extend individual AI workflows into team and company concerns: rollout, adoption,
permissions, privacy, security, compliance, governance, integration, reliability, change management,
ownership, support, measurement, and scaling beyond a pilot. Prefer useful specificity and
target-audience relevance over novelty or raw popularity.

## Start every request

1. Identify the requested action, topic, or project from the user's words.
2. Query the Ideas and Projects data sources and fetch the relevant pages and comments.
3. If the user says “continue” or asks what is next, infer the action from the artifacts below.
4. If more than one action or project is plausible, ask one focused question listing the options.
   Do not ask the user to repeat information already in Notion.

Infer the next action as follows:

- A new topic, trend discovery, or supporting signals are needed: **ideation**.
- A selected idea has no substantive `Video Outline`: **develop outline**.
- A substantive outline exists but no creator draft exists: stop at the human writing gate.
- A creator-written draft needs critique: **review**.
- A substantive creator draft needs diagrams: **visuals**.

## Notion conventions

Ideas use `Name`, `Status`, `Topic`, `Profile`, and `Created`. Use only the existing status options:
`New`, `Selected`, `Parked`, and `Discarded`.

Projects use `Name`, `Status`, and `Profile`. Use only `In progress`, `Complete`, and `Parked`.
Keep a project `In progress` while developing it. Mark it `Complete` or `Parked` only when the creator
decides.

For future work, create only these child pages as needed: `Video Outline`, `Video Draft`, and
`Visuals`. Do not create `Brief`, `Research`, any article page, or a separate review page. Preserve
existing legacy pages and projects; read them when useful, but do not update them unless asked.

Keep agent feedback under a `## Review` heading on `Video Draft`. Everything above that heading is
creator-owned. Read the full page and its comments before editing it, and confine review updates to
the section below that heading. Never replace or edit the creator's draft while reviewing it.

Prefer targeted Markdown updates. Full replacement is acceptable only for a leaf document page.
Never replace the Content Factory root, a database, or a project parent page, and never enable
content deletion to force an update.

## Source collection

Before ideation or outline development, inspect available skills for source-specific search or
scraping tools. Prefer specialist tools when they provide better transcripts, authorship, dates, or
visible engagement metrics. Fall back to web search for uncovered sources.

Research a small number of specific transcripts, posts, and articles deeply enough to understand
their actual content. Preserve the original URL, creator or publisher, publication date, collection
date, and visible engagement metrics when available. Prefer primary sources. Clearly distinguish
facts, source claims, and agent inference. Verify quotations against the source. Link every material
factual claim to supporting evidence, and note uncertainty or conflicting evidence.

Keep research proportional. A few representative credible examples are normally enough. Go deeper
only when the creator asks or a central claim cannot otherwise be supported.

## Ideation

Goal: add sourced ideas and supporting signals to the Ideas database.

1. Search existing ideas and projects before collecting anything new.
2. Look for repeated audience questions, unusual claims, strong engagement, recent developments,
   contradictions, and gaps in existing coverage.
3. Deduplicate related findings. Add support to an existing idea instead of creating a duplicate.
4. Create an Ideas item with `Status: New`, metadata properties, and concise sections for the signal,
   why it matters now, audience problem, possible angle, sources, open questions, related work, and
   agent assessment.
5. End with the strongest candidates. Do not select an idea or create a project unless the creator
   explicitly delegates selection.

## Develop outline

Goal: turn a selected idea and specific source material into a potential video structure.

1. Use the idea selected by the creator. If none is selected, present the strongest relevant options
   and ask the creator to choose.
2. Mark the idea `Selected`. Create one Projects item with `Status: In progress` and a `Video Outline`
   child page. Do not create a separate brief or research page.
3. Read a small set of relevant transcripts, posts, and articles. Investigate audience questions,
   concrete examples, disagreements, limitations, gaps in existing coverage, and the evidence needed
   for the video's central claim.
4. Put the useful source analysis directly into `Video Outline`: viewer promise, potential titles and
   hook, audience problem and angle, source notes with links and what each source supports, ordered
   segments with evidence and examples, demonstrations and visual opportunities, gaps or
   uncertainty, payoff, and next step.
5. Do not create draft prose or a script. Stop at the human writing gate and state that the creator's
   `Video Draft` is next.

## Review

Goal: give specific editorial feedback without replacing the creator's voice.

1. Read `Video Draft`, `Video Outline`, their comments, and the linked primary sources needed to
   verify material claims.
2. Preserve everything above `## Review`. Add or update feedback only below that heading on the same
   `Video Draft` page. Never create a separate review page.
3. Quote or precisely identify the affected passage. Separate factual errors, structural problems,
   missing material, and optional editorial preferences. Explain the issue and suggest a direction.
4. Assess click-promise alignment, hook, pacing, audience assumptions, open loops, demonstrations,
   transitions, re-hooks, payoff, next step, and factual support.
5. Include a small number of fitting title or hook options when useful and disclose overpromising
   risk. Prioritize the smallest set of changes that materially improves the video.

## Visuals

Goal: create explanatory diagrams from the creator's substantive video draft.

1. Read `Video Draft`, including its review section. Choose concepts where a diagram explains a
   relationship, process, comparison, or state change better than narration or ordinary B-roll.
2. Create or update `Visuals` with the draft location, concept, diagram plan, and output filename.
3. Create one focused Excalidraw diagram per concept when the environment supports it. Otherwise
   create a reviewable SVG or image. Attach finished assets to the Visuals page when possible; if an
   upload tool is unavailable, return the file to the creator and record its filename on the page.
4. Prefer fewer elements, black strokes, no color fills, minimal text, and strong hierarchy. Use all
   caps only for the H1 title. Keep text inside its boxes. Route arrows through open space so they do
   not cross shapes or text.
5. Do not create decorative diagrams.

## Finish every action

- State which Notion items and pages were created or updated.
- State unresolved factual questions and creator decisions.
- Do not silently proceed into another action.
- Do not publish content or alter creator-owned prose unless explicitly requested.
