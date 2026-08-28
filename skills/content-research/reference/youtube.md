# YouTube Research

Use this workflow to collect reliable YouTube evidence for the `content-research` skill. Return the
short deliverable defined in `SKILL.md`; do not turn collection details into a separate report.

## Define the Search

Set the topic, target viewer, language or region, time window, and desired sample size. If the user
sets a subscriber threshold or other filter, verify it rather than silently relaxing it.

Search with several narrow queries. Search both channels and videos, then combine recent,
high-view, and topic-matched videos instead of trusting one ranking. Riley Brown, Peter Yang, Simon
Scrapes, and Nate Herk are useful starting points when relevant, not a required or complete list.
Deduplicate channels by channel ID and videos by video ID.

## Choose a Collection Method

Check only whether supported credentials exist; never display their values:

- `YOUTUBE_API_KEY` for official discovery, metadata, channel statistics, and comments
- `SUPADATA_API_KEY` for YouTube data and transcripts
- `SCRAPECREATORS_API_KEY` for YouTube data, transcripts, and comments

If no key exists, continue with public YouTube pages and captions. Do not require a paid provider.
Use Supadata or ScrapeCreators only when its key is already configured or the user chooses it after
being told it may incur cost. Provider behavior, pricing, and quotas change; check current provider
documentation before a large collection run.

## Free Public Workflow

For a modest, manually verifiable sample:

1. Search YouTube with several narrow queries and retain canonical channel and video URLs.
2. Inspect each channel's public page for its handle, visible subscriber count, and recent or popular
   videos. Mark hidden or unavailable counts as `unverified`.
3. Inspect each selected video for its title, channel, publication date, description, visible
   metrics, and comments. Treat unavailable fields as missing data.
4. Use YouTube's **Show transcript** interface when available. Retain useful timestamps and note
   whether captions appear creator-provided or auto-generated.
5. Sample both top and newest comments. Top comments often reveal resonance, objections, and shared
   language; newest comments can reveal current questions or stale advice. Read replies only when
   they materially change the interpretation.

For low-volume automation, `youtube-transcript-api` can retrieve public caption tracks without an
API key. It relies on unofficial YouTube behavior, may be blocked from cloud IPs, and can break; fall
back to the public transcript interface. It does not provide discovery, metadata, or comments.

Do not scrape undocumented endpoints at scale, bypass access controls, download video media, or
claim comprehensive transcript or comment coverage.

## Official YouTube Data API

When `YOUTUBE_API_KEY` exists, prefer the official API for scalable public data:

| Need | Method | Notes |
|---|---|---|
| Discover channels or videos | `search.list` | Use `q`, `type`, and pagination |
| Verify a channel | `channels.list` | Request `snippet,statistics,contentDetails` |
| Enumerate uploads | `playlistItems.list` | Page through the uploads playlist |
| Fetch video details | `videos.list` | Request `snippet,contentDetails,statistics` |
| Fetch top-level comments | `commentThreads.list` | Use `order=relevance` and `order=time` intentionally |
| Fetch remaining replies | `comments.list` | Use `parentId` when inline replies are incomplete |

Official documentation:

- https://developers.google.com/youtube/v3/docs/search/list
- https://developers.google.com/youtube/v3/docs/channels/list
- https://developers.google.com/youtube/v3/docs/playlistItems/list
- https://developers.google.com/youtube/v3/docs/videos/list
- https://developers.google.com/youtube/v3/docs/commentThreads/list
- https://developers.google.com/youtube/v3/docs/comments/list
- https://developers.google.com/youtube/v3/determine_quota_cost

Subscriber counts can be rounded or unavailable. Comments can be disabled. Inline replies can be
incomplete. Record retrieval dates for changing metrics and report these gaps. The official
`captions.download` method requires authorization from someone who can edit the video, so it is not
a transcript solution for arbitrary creators' videos.

## Transcript and Combined Providers

### Supadata

Use Supadata for timestamped transcripts when `SUPADATA_API_KEY` exists. Prefer native captions.
Generated transcription may cost more and introduce errors; tell the user and obtain agreement
before using it. Long-running requests may return a job to poll.

Supadata can also search YouTube and retrieve channel, video, and metadata records, but it does not
provide YouTube comments. Documentation: https://docs.supadata.ai/llms.txt

### ScrapeCreators

Use ScrapeCreators when `SCRAPECREATORS_API_KEY` exists and one provider is useful for search,
channel data, video metadata, public transcripts, and comments. Relevant endpoints include:

| Need | Endpoint |
|---|---|
| Search | `GET /v1/youtube/search` |
| Channel verification | `GET /v1/youtube/channel` |
| Channel videos | `GET /v1/youtube/channel-videos` |
| Video metadata | `GET /v1/youtube/video` |
| Transcript | `GET /v1/youtube/video/transcript` |
| Comments | `GET /v1/youtube/video/comments` |
| Replies | `GET /v1/youtube/video/comment/replies` |

Each page can consume credits. Public captions may be absent, and restricted content can fail.
Documentation: https://docs.scrapecreators.com/

## Evidence to Collect

For each candidate, retain enough source data to verify the final research:

- canonical video URL and ID
- creator, channel URL or ID, title, description, publication date, and duration
- visible views, likes, and comment count with retrieval date
- timestamped transcript or caption status
- a small, intentional sample of relevant top and newest comments

Use the transcript or video as primary evidence and the description as supporting context. Do not
infer the argument from the title. Separate creator claims from verified facts and agent inference.
Deduplicate mirrored clips, compilations, and repeated uploads. Prefer a diverse source set over
many nearly identical channels. Keep excerpts short and link to the source rather than reproducing
large transcript or comment sections.

Report inaccessible videos, missing transcripts or comments, generated-transcript use, hidden
metrics, and any filters that could not be verified. Then format the findings using the concise
source, hook, content, comment-angle, and synthesis structure in `SKILL.md`.
