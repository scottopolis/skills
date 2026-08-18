---
name: researching-youtube-ideas
description: Researches YouTube channels and videos to develop evidence-backed content ideas using channel metrics, metadata, transcripts, and comments. Use for competitor research, topic discovery, audience-question mining, or YouTube video ideation without downloading videos.
---

# Researching YouTube Ideas

Build a traceable research corpus before proposing video ideas. Use public data and APIs rather than downloading video files.

## Workflow

1. Define the niche, target viewer, language/region, time window, minimum subscriber count, and desired number of channels and ideas. Default the subscriber threshold to the user's stated value; never silently relax it.
2. Check only whether supported environment variables exist; never print secret values. Use `YOUTUBE_API_KEY`, `SUPADATA_API_KEY`, or `SCRAPECREATORS_API_KEY`.
3. Choose a collection stack from [reference/providers.md](reference/providers.md):
   - If no supported API key exists, use the free no-key workflow. Research through public YouTube search, channel, video, transcript, and comment pages; optionally use `youtube-transcript-api` for low-volume public-caption retrieval. Do not require the user to obtain a key or use a paid provider.
   - If `YOUTUBE_API_KEY` exists, prefer the YouTube Data API for discovery, channel verification, metadata, and comments. Use free public transcripts or `youtube-transcript-api` unless another transcript provider key already exists.
   - Use Supadata or ScrapeCreators only when its corresponding key already exists or the user explicitly chooses that provider after being told about possible cost.
   - If API-based scale or reliability would materially improve the result, mention the optional YouTube API key setup instructions in the provider reference while continuing with free methods.
4. Discover candidate channels with multiple narrow queries. Search both channel results and videos, deduplicate by immutable channel ID, then fetch each channel's current statistics.
5. Keep only channels with a verifiable `subscriberCount >= threshold`. Record the retrieval date and source. Mark hidden or unavailable subscriber counts as `unverified`; do not treat them as passing.
6. Build a video sample per accepted channel. Combine recent uploads, high-view uploads, and query-matched videos rather than relying on one ranking. Exclude Shorts or live streams when they do not fit the requested format.
7. Collect canonical video URL, ID, channel ID, title, full description, publication time, duration, views, likes, comment count, transcript language, timestamped transcript, and sampled comments. Preserve raw responses or source URLs so results can be audited.
8. Fetch transcripts without downloading media. Try YouTube's public transcript view or another existing/native caption source first. Only use generated transcription after telling the user it may cost money and introduce recognition errors, and obtaining agreement.
9. Sample comments intentionally:
   - Top comments reveal resonance, objections, and shared language.
   - Newest comments reveal current questions and whether advice has gone stale.
   - Fetch reply pages only for threads that materially affect the analysis.
   - Treat disabled or unavailable comments as missing data, not negative audience sentiment.
10. Normalize the corpus before analysis. Separate source text from model interpretation and retain transcript timestamps and comment IDs where available.
11. Derive ideas from repeated evidence across titles, transcripts, and comments. For each idea identify:
    - Target viewer and business problem
    - Proposed title and opening hook
    - Distinct promise or angle
    - Evidence from at least two videos or one video plus its audience comments
    - Saturation risk and how the proposed treatment differs
    - Relevant source links and transcript timestamps
12. Report collection limits, inaccessible videos, missing transcripts/comments, generated-transcript usage, and subscriber counts that could not be verified.

## Research Discipline

- Do not infer a video's argument from its title alone. Use the transcript as the primary source and the description as supporting context.
- Distinguish creator claims from independently verified facts.
- Do not present views, likes, subscriber counts, or rankings without a retrieval date; these values change.
- Deduplicate mirrored clips, compilations, and repeated uploads before counting themes.
- Prefer a diverse channel set over many near-identical channels from one search query.
- Keep excerpts short and analytical. Link to source videos instead of reproducing transcripts or large comment sets.
- Follow provider terms, quotas, privacy rules, and YouTube's applicable terms. Collect only public content and stop on access restrictions rather than bypassing them.
- Use paths relative to the active workspace or an output location chosen by the user. Never assume a particular home directory, username, repository, or operating system.

## Default Deliverables

Return:

1. **Method and coverage** — providers, retrieval date, queries, filters, and known gaps.
2. **Qualified channels** — channel name/URL/ID, subscriber count, why it is relevant, and evidence source.
3. **Video corpus** — title, URL, date, metrics, transcript status, and comment coverage.
4. **Audience signals** — repeated questions, objections, desired outcomes, terminology, and stale advice.
5. **Video ideas** — ranked concepts with hook, differentiation, and source evidence.

When saving data, use JSONL for raw records, CSV for sortable inventories, and Markdown for the synthesis. Never store API keys in output files.
