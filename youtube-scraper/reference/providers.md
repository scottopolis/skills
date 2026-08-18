# YouTube Scraper Providers

Provider behavior and pricing can change. Re-check linked documentation before a large collection run.

## Provider Selection

Check for supported environment variables without displaying their values. If no key is configured, start with the free no-key workflow below. Do not block research on credentials and do not send the user to a paid provider by default.

Use an API when the matching key already exists, the volume requires it, or the user explicitly chooses it:

- `YOUTUBE_API_KEY`: official discovery, metadata, channel statistics, and comments.
- `SUPADATA_API_KEY`: transcript and YouTube data service; usage may consume paid credits.
- `SCRAPECREATORS_API_KEY`: combined YouTube data and transcript service; usage may consume paid credits.

## Free Workflow Without an API Key

Use public YouTube pages and publicly available captions. This workflow is best for modest research sets where every source can be checked directly.

1. Search on YouTube with several narrow queries. Record channel and video URLs as evidence.
2. Open each channel's public page to inspect its handle, subscriber count when visible, and recent or popular videos. Mark hidden or unavailable counts as `unverified`.
3. Open each selected video's public page to record its title, channel, publication date, description, visible metrics, and top or newest comments. Treat unavailable fields as missing data.
4. Use YouTube's **Show transcript** interface when available. Preserve timestamps and note whether captions appear creator-provided or auto-generated.
5. For low-volume automation, optionally use `youtube-transcript-api` to retrieve public caption tracks. It requires no API key but relies on unofficial YouTube behavior, so fall back to the public transcript interface if it fails.
6. Keep the sample small enough to verify manually. Report where public pages, transcripts, comments, or metrics were inaccessible rather than bypassing restrictions.

Do not scrape undocumented endpoints at scale, bypass access controls, download video media, or claim complete comment coverage from a manually sampled page.

## YouTube Data API Key Setup

The YouTube Data API provides an official, quota-limited way to automate public metadata and comment collection. To create a key:

1. Go to the [Google Cloud Console](https://console.cloud.google.com/) and sign in with a Google account.
2. Open the project drop-down menu at the top, then select **New Project**.
3. Name the project and select **Create**.
4. Open the left menu, select **APIs & Services**, then select **Library**.
5. Search for **YouTube Data API v3**, open it, and select **Enable**.
6. Return to **APIs & Services**, then select **Credentials**.
7. Select **+ Create Credentials**, then choose **API key**.
8. Restrict the key to the YouTube Data API v3 and to the applications or IP addresses that will use it when practical.
9. Store it securely and expose it to the research process as `YOUTUBE_API_KEY`. Never commit it or paste it into research output.

Google applies daily API quotas. Check the current quota documentation before large runs.

## API-Based Split: YouTube Data API + Supadata

Use the official YouTube Data API for public metadata and comments, then Supadata only for transcript text. This keeps most collection on an official API while avoiding the official captions restriction for competitor videos.

### YouTube Data API v3

Authentication: API key for public read requests (`YOUTUBE_API_KEY`).

Useful methods:

| Need | Method | Important fields/notes |
|---|---|---|
| Discover channels/videos | `search.list` | `q`, `type`, `maxResults<=50`, `pageToken`; search has a dedicated quota bucket, so check the current quota page |
| Verify channel | `channels.list` | Request `snippet,statistics,contentDetails`; use `statistics.subscriberCount` and the uploads playlist in `contentDetails.relatedPlaylists.uploads` |
| Enumerate uploads | `playlistItems.list` | Page through the uploads playlist, up to 50 items per request |
| Fetch video details | `videos.list` | Batch up to 50 IDs; request `snippet,contentDetails,statistics` for titles, descriptions, duration, and public metrics |
| Fetch top-level comments | `commentThreads.list` | `videoId`, `part=snippet,replies`, `order=relevance|time`, `textFormat=plainText`, `maxResults<=100` |
| Fetch all replies | `comments.list` | Use `parentId` when inline replies are incomplete |

Official documentation:

- Search: https://developers.google.com/youtube/v3/docs/search/list
- Channels: https://developers.google.com/youtube/v3/docs/channels/list
- Playlist items: https://developers.google.com/youtube/v3/docs/playlistItems/list
- Videos: https://developers.google.com/youtube/v3/docs/videos/list
- Comment threads: https://developers.google.com/youtube/v3/docs/commentThreads/list
- Replies: https://developers.google.com/youtube/v3/docs/comments/list
- Quotas: https://developers.google.com/youtube/v3/determine_quota_cost

Limitations:

- Subscriber counts can be rounded, and some channels may not expose a usable count. Record the returned value and retrieval date.
- Public comments may be disabled. `commentThreads.list` then returns `403 commentsDisabled`.
- `part=replies` does not guarantee every reply. Compare the returned replies with `totalReplyCount` and call `comments.list` when complete replies matter.
- The official `captions.download` method costs substantial quota and requires OAuth authorization from a user who can edit the video. It is therefore not a transcript solution for arbitrary competitor videos: https://developers.google.com/youtube/v3/docs/captions/download

### Supadata Transcript API

Authentication: `x-api-key: $SUPADATA_API_KEY`.

```bash
curl --get 'https://api.supadata.ai/v1/transcript' \
  -H "x-api-key: $SUPADATA_API_KEY" \
  --data-urlencode 'url=https://www.youtube.com/watch?v=VIDEO_ID' \
  --data-urlencode 'mode=native'
```

Use `text=false` (the default) for timestamped chunks. A direct result contains `content[]` with `text`, `offset`, and `duration`; long or generated jobs may return HTTP 202 with a `jobId` to poll at `/v1/transcript/{jobId}`.

Modes:

- `native`: existing captions only; lowest cost and no generated-media transcription.
- `auto`: native first, then AI generation if needed.
- `generate`: always AI transcription; use only when requested or justified.

Current documentation states 1 credit for a native transcript and 2 credits per generated minute. Public, completed videos only; private, membership-only, age-restricted, heavily geoblocked, and active live streams can fail.

Docs: https://docs.supadata.ai/get-transcript

## Single-Provider Option: ScrapeCreators

Use when comments and transcripts must come through one service. Authentication is `x-api-key: $SCRAPECREATORS_API_KEY`; base URL is `https://api.scrapecreators.com`.

| Need | Endpoint | Key parameters/output |
|---|---|---|
| Search | `GET /v1/youtube/search` | `query`, optional `type`, `sortBy`, `uploadDate`, `continuationToken`; returns videos/channels and next token |
| Channel verification | `GET /v1/youtube/channel` | One of `channelId`, `handle`, `url`; returns `subscriberCount` |
| Channel videos | `GET /v1/youtube/channel-videos` | `channelId` or `handle`, optional `sort`, `continuationToken` |
| Video metadata | `GET /v1/youtube/video` | `url`; returns title, description, channel, dates, duration, and public metrics |
| Transcript | `GET /v1/youtube/video/transcript` | `url`, optional `language`; returns timestamped segments and plain text when public captions exist |
| Comments | `GET /v1/youtube/video/comments` | `url`, optional `order=top|newest`, `continuationToken` |
| Replies | `GET /v1/youtube/video/comment/replies` | Start with a comment's `repliesContinuationToken`, then continue with response tokens |

Each page is a separate request. Responses expose `credits_charged` and `credits_remaining`; cache-aware endpoints may be free on a qualifying cache hit. The transcript endpoint depends on public captions and does not provide AI fallback. Age-restricted or otherwise non-public content can fail.

Docs: https://docs.scrapecreators.com/

## Supadata-Only Option

Supadata can cover search, channel verification, channel video IDs, video metadata, and transcripts, but it does not currently provide YouTube comments.

| Need | Endpoint |
|---|---|
| Search | `GET https://api.supadata.ai/v1/youtube/search` |
| Channel | `GET https://api.supadata.ai/v1/youtube/channel` |
| Channel videos | `GET https://api.supadata.ai/v1/youtube/channel/videos` |
| Video metadata | `GET https://api.supadata.ai/v1/metadata` |
| Transcript | `GET https://api.supadata.ai/v1/transcript` |

Authentication: `x-api-key: $SUPADATA_API_KEY`. Search can automatically paginate with `limit`, but each fetched page consumes a credit. Manual pagination uses `nextPageToken`.

Docs index: https://docs.supadata.ai/llms.txt

## Free, Unofficial Transcript Library

`youtube-transcript-api` can fetch public caption tracks without downloading video media or requiring an API key. Use it only for low-volume experiments after checking whether its unofficial access pattern is acceptable for the user's use case.

Tradeoffs:

- Relies on undocumented YouTube behavior and can break when YouTube changes it.
- Cloud-hosted IPs are commonly rate-limited or blocked.
- No transcript when captions are unavailable and no AI fallback.
- It does not solve channel discovery, metadata, or comments.

Project: https://github.com/jdepoix/youtube-transcript-api

Do not build a production research workflow around undocumented endpoints when an approved API is available.
