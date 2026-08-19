---
name: developing-content
description: Runs a Notion-based content factory for AI business articles and YouTube videos. Use for idea discovery, research, article and video outlines, draft review, explanatory visuals, continuing a content project, or deciding the next stage.
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

Run the Content Factory entirely in Notion. It has five agent actions: **ideation**, **research**,
**outline**, **review**, and **visuals**. Human writing is the gate between outline and review.

## Workspace

Use these stable IDs instead of discovering duplicate databases by title:

- Content Factory root: `3c1bebc7-23c3-815e-b7d6-ea7518b07858`
- Ideas database: `3c1bebc7-23c3-81ef-bf32-f6cf57b95833`
- Ideas data source: `3c1bebc7-23c3-8196-9bda-000bd5f9a53d`
- Projects database: `3c1bebc7-23c3-81ec-80c3-f0f09419af14`
- Projects data source: `3c1bebc7-23c3-81ed-b3d3-000be85c4c0b`

Notion is the source of truth for ideas, briefs, research, outlines, creator writing, reviews, and
visual plans. Do not create or update a parallel repository copy.

## Audience and roles

The primary audience is business owners and team leaders over 40 at medium-to-large businesses.
Build credibility around implementing AI in real organizations, not maximum generic reach.

The creator owns idea selection, angle, thesis, opinions, final wording, and publication decisions.
The creator writes article drafts and video scripts. The agent discovers ideas, researches selected
ideas, creates separate outlines, reviews creator writing, proposes hooks during review, and creates
explanatory visuals. Do not write or rewrite article prose or video scripts unless explicitly asked.

When relevant, extend individual AI workflows into team and company concerns: rollout, adoption,
permissions, privacy, security, compliance, governance, integration, reliability, change management,
ownership, support, measurement, and scaling beyond a pilot. Prefer useful specificity and
target-audience relevance over novelty or raw popularity.

## Start every request

1. Identify the requested stage, topic, or project from the user's words.
2. Query the Ideas and Projects data sources and fetch the relevant pages and comments.
3. If the user says “continue” or asks what is next, infer the stage from the artifacts below.
4. If more than one stage or project is plausible, ask one focused question listing the options. Do
   not ask the user to repeat information already in Notion.

Infer the next stage as follows:

- New topic, trend discovery, or supporting signals needed: **ideation**.
- Selected idea without a complete Brief and Research: **research**.
- Substantive research without both outlines: **outline**.
- Both outlines exist but no substantive creator draft or script exists: stop at the human writing gate.
- A creator-written draft or script needs critique: **review**.
- A substantive video script needs diagrams: **visuals**.

## Notion conventions

Ideas use `Name`, `Status`, `Topic`, `Profile`, and `Created`. Use only the existing status options:
`New`, `Selected`, `Parked`, and `Discarded`.

Projects use `Name`, `Stage`, and `Profile`. Use only the existing stages: `Research`, `Outline`,
`Writing`, `Review`, `Visuals`, `Complete`, and `Parked`.

Create only the child pages required by the current stage. Use these names consistently: `Brief`,
`Research`, `Article Outline`, `Video Outline`, `Article Draft`, `Video Script`, `Article Review`,
`Video Review`, and `Visuals`.

Read comments before editing a page. Prefer targeted Markdown updates. Full replacement is acceptable
only for a leaf document page. Never replace the Content Factory root, a database, or a project
parent page, and never enable content deletion to force an update.

## Source collection

Before ideation or research, inspect available skills for source-specific search or scraping tools.
Prefer specialist tools when they provide better transcripts, authorship, dates, or visible
engagement metrics. Fall back to web search for uncovered sources.

Preserve the original URL, creator or publisher, publication date, collection date, and visible
engagement metrics when available. Prefer primary sources. Clearly distinguish facts, source claims,
and agent inference. Verify quotations against the source. Link every material factual claim to
supporting evidence, and note uncertainty or conflicting evidence.

Keep research proportional. A few representative credible examples are normally enough. Go deeper
only when the creator asks or a central claim cannot otherwise be supported.

## Ideation

Goal: add sourced ideas and supporting signals to the Ideas database.

1. Search existing ideas and projects before collecting anything new.
2. Look for repeated audience questions, unusual claims, strong engagement, recent developments,
   contradictions, and gaps in existing coverage.
3. Deduplicate related findings. Add support to an existing idea instead of creating a duplicate.
4. Create an Ideas item with `Status: New`, metadata properties, and sections for the signal, why it
   matters now, audience problem, possible angle, sources, open questions, related work, and agent
   assessment.
5. End with the strongest candidates. Do not select an idea or create a project unless the creator
   explicitly delegates selection.

## Research

Goal: turn the creator's selected idea into a reliable shared research pack.

1. Use the idea selected by the creator. If none is selected, present the strongest relevant options
   and ask the creator to choose.
2. Mark the idea `Selected`. Create one Projects item at Stage `Research` with `Brief` and `Research`
   child pages.
3. Research audience questions, primary evidence, representative competing coverage, examples,
   counterexamples, disagreements, limitations, and unresolved questions.
4. The Brief holds profile, source idea, audience, problem, provisional thesis and promise,
   differentiation, intended belief or action, and boundaries. Label undecided creator choices.
5. Research holds a summary, assumptions, audience questions, existing coverage, an evidence table
   with confidence, counterarguments, limitations, opportunities, visual possibilities, unresolved
   questions, and sources.
6. Do not create outlines or creator prose during this stage.

## Outline

Goal: produce independent article and video structures from the same evidence.

1. Read Brief, Research, comments, and creator decisions. Return to research if evidence for a
   central section is missing.
2. Create `Article Outline`: reader promise, headline directions, opening, ordered argument,
   evidence and examples per section, transitions, conclusion, and overpromising risks.
3. Separately create `Video Outline`: viewer promise, title concepts, hook strategy, ordered segments,
   demonstrations, visual opportunities, transitions or re-hooks, payoff, and next step.
4. Do not mechanically convert one outline into the other. Do not create draft prose or a script.
5. Set the project Stage to `Writing` and state that creator writing is the next gate.

## Review

Goal: give specific editorial feedback without replacing the creator's voice.

1. Determine whether the article, video, or both should be reviewed. Read the creator draft or
   script, corresponding outline, Brief, Research, and existing comments.
2. Verify material factual claims against Research and primary sources when needed.
3. Put findings in `Article Review` or `Video Review`; do not silently edit creator writing.
4. Quote or precisely identify the affected passage. Separate factual errors, structural problems,
   missing material, and optional editorial preferences. Explain the issue and suggest a direction.
5. For articles, assess promise delivery, argument, evidence, organization, clarity, examples, voice,
   opening, conclusion, and headline alignment.
6. For videos, assess click-promise alignment, hook, pacing, audience assumptions, open loops,
   demonstrations, transitions, re-hooks, payoff, and next step.
7. Include several fitting headline/opening or title/hook options and disclose overpromising risk.
8. Prioritize the smallest set of changes that materially improves the piece. Set Stage to `Review`.

## Visuals

Goal: create explanatory diagrams from the creator's substantive video script.

1. Read Video Script and Video Review. Choose concepts where a diagram explains a relationship,
   process, comparison, or state change better than narration or ordinary B-roll.
2. Update `Visuals` with the script location, concept, diagram plan, and output filename.
3. Create one focused Excalidraw diagram per concept when the environment supports it. Otherwise
   create a reviewable SVG or image. Attach finished assets to the Visuals page when possible; if an
   upload tool is unavailable, return the file to the creator and record its filename on the page.
4. Prefer fewer elements, black strokes, no color fills, minimal text, and strong hierarchy. Use all
   caps only for the H1 title. Keep text inside its boxes. Route arrows through open space so they do
   not cross shapes or text.
5. Do not create decorative diagrams. Set Stage to `Visuals`.

## Finish every action

- State which Notion items and pages were created or updated.
- State unresolved factual questions and creator decisions.
- Do not silently proceed into another stage.
- Do not publish content or alter creator-owned prose unless explicitly requested.
