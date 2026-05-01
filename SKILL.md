---
name: social-poster
description: Draft, schedule, and track social media posts across X/Twitter and Telegram channels with hashtag research and content calendar integration
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  tags:
    - social-media
    - twitter
    - telegram
    - scheduling
    - obsidian
    - content-calendar
    - cron
  related_skills:
    - xurl
    - telegram-bot
    - obsidian-vault
---

# Social Poster

Draft, schedule, and track posts across X/Twitter and Telegram channels. Includes hashtag research, tone presets, content calendar in Obsidian, and cron-based scheduled posting.

## Overview

Social Poster streamlines your social media workflow by providing a unified interface for X/Twitter (via the `xurl` skill) and Telegram channels. It handles drafting with tone presets, hashtag research, scheduling, content calendar management, and engagement tracking — all integrated with your Obsidian vault.

## Commands

### `/post-draft <platform> <message>`
Create a post draft with a tone preset.

**Arguments:**
- `<platform>` — `twitter`, `telegram`, or `both`
- `<message>` — The post content (use quotes for multi-word)

**Options:**
- `--tone <preset>` — `professional` (default), `casual`, or `hype`
- `--hashtags <count>` — Auto-generate N relevant hashtags (default: 3)
- `--media <path>` — Attach an image or video
- `--thread` — Compose a thread (Twitter only)

**Examples:**
```
/post-draft twitter "Launching our new feature this week!" --tone professional --hashtags 5
/post-draft telegram "Check out our latest update" --tone casual --media ~/image.png
/post-draft both "Big announcement coming soon..." --tone hype
```

### `/post-schedule <draft-id> <datetime>`
Schedule a drafted post for a specific time.

**Arguments:**
- `<draft-id>` — ID from `/post-draft`
- `<datetime>` — ISO 8601 datetime (e.g., `2025-01-15T09:00`)

**Example:**
```
/post-schedule draft-0042 2025-01-15T09:00
```

### `/post-publish <draft-id>`
Publish a draft immediately (no scheduling).

**Example:**
```
/post-post draft-0042
```

### `/post-list [status]`
List posts by status: `draft`, `scheduled`, `published`, or `all` (default).

**Example:**
```
/post-list scheduled
```

### `/post-edit <draft-id> [field] [value]`
Edit an existing draft's content, tone, hashtags, or media.

**Examples:**
```
/post-edit draft-0042 content "Updated message text"
/post-edit draft-0042 tone casual
/post-edit draft-0042 hashtags 5
```

### `/post-cancel <draft-id>`
Cancel a scheduled post.

### `/hashtag-research <topic>`
Research popular and relevant hashtags for a topic.

**Example:**
```
/hashtag-research AI startups
```

### `/engagement-track <post-id>`
Check engagement metrics for a published post.

**Example:**
```
/engagement-track post-0042
```

### `/calendar [month]`
Show the content calendar for the current or specified month.

**Examples:**
```
/calendar
/calendar 2025-02
```

## Tone Presets

### Professional
- Clear, concise language
- Avoids slang and excessive punctuation
- Strategic hashtag placement (1-3 hashtags)
- Ends with a call to action
- **Template:** `{announcement}. {context/key_detail}. {call_to_action} {hashtags}`

### Casual
- Conversational tone, contractions encouraged
- Emojis sparingly (1-2 per post)
- Hashtags woven into text or at end (2-5 hashtags)
- **Template:** `{hook} 🤔 {casual_body} {hashtags}`

### Hype
- Energy and enthusiasm, exclamation points
- Emojis liberally (2-4 per post)
- ALL CAPS for key phrases
- Hashtags front-loaded (3-7 hashtags)
- **Template:** `{hype_opener} 🔥 {bold_claim}!! {excitement} {hashtags}`

## Content Calendar

Posts are tracked in an Obsidian vault calendar at `2-Projects/Social Media/Calendar/`:

```markdown
---
title: "Social Media Calendar — January 2025"
type: calendar
month: 2025-01
tags: [social-media, calendar]
---

## Week 1 (Jan 1-5)

| Date | Platform | Status | Content |
|------|----------|--------|---------|
| 2025-01-02 09:00 | Twitter | ✅ Published | Launch announcement |
| 2025-01-03 12:00 | Telegram | 📅 Scheduled | Feature highlight |

## Upcoming Drafts
- [ ] draft-0043: Twitter thread on AI trends
- [ ] draft-0044: Telegram community poll

## Engagement Summary
- Total impressions: 12,400
- Avg engagement rate: 4.2%
- Top performing post: post-0039 (5.1% rate)
```

### Individual Post Notes

Each published post gets a note at `2-Projects/Social Media/Posts/`:

```markdown
---
title: "Post — Launch Announcement"
date: 2025-01-15
platform: twitter
status: published
draft_id: draft-0042
post_id: "1234567890"
tone: professional
hashtags: ["#AI", "#Startup", "#Launch"]
---

## Content
Launching our new AI-powered feature this week! Start your free trial today. #AI #Startup #Launch

## Engagement (as of 2025-01-16)
- Likes: 142
- Retweets: 38
- Replies: 12
- Impressions: 8,200
- Engagement rate: 2.3%

## Notes
Follow-up: Share user testimonials next week.
```

## Hashtag Research

The `/hashtag-research` command generates hashtag suggestions:

```
/hashtag-research AI startups

→ 🔍 Researching hashtags for: "AI startups"

**Top Hashtags (by relevance):**
1. #AI — 2.1M posts/day, high competition
2. #Startups — 890K posts/day, high competition
3. #AIStartups — 45K posts/day, medium competition
4. #MachineLearning — 1.2M posts/day, high competition
5. #TechStartups — 120K posts/day, low competition ← 🎯 sweet spot

**Niche Hashtags (higher engagement):**
6. #AIBuilders — 8K posts/day, low competition
7. #Founders — 55K posts/day, medium competition

**Recommended mix:** 2 high-reach + 2 niche + 1 branded
```

## Scheduled Posting (Cron)

Social Poster supports cron-based scheduling for recurring posts.

### Setup Cron Schedule
```
/post-cron add "0 9 * * 1-5" twitter "Daily tech tip" --tone professional
/post-cron add "0 12 * * 3" telegram "Weekly community update" --tone casual
```

### Manage Cron Jobs
```
/post-cron list           # Show all scheduled cron jobs
/post-cron pause <id>     # Pause a cron job
/post-cron resume <id>    # Resume a paused cron job
/post-cron remove <id>    # Delete a cron job
```

### Cron Configuration

Cron jobs are stored in `~/.hermes/skills/social-media/social-poster/crontab.json`:

```json
{
  "jobs": [
    {
      "id": "cron-001",
      "schedule": "0 9 * * 1-5",
      "platform": "twitter",
      "template": "Daily tech tip",
      "tone": "professional",
      "enabled": true,
      "last_run": "2025-01-15T09:00:00Z",
      "next_run": "2025-01-16T09:00:00Z"
    }
  ]
}
```

## Engagement Tracking

After publishing, track engagement with `/engagement-track`:

```
/engagement-track post-0042

→ 📊 Engagement Report for post-0042

**Platform:** Twitter
**Published:** 2025-01-15 09:00
**Content:** "Launching our new AI-powered feature..."

┌─────────────────────────────┐
│  Likes      ██████████ 142  │
│  Retweets   ██████     38   │
│  Replies    ██         12   │
│  Impressions █████████ 8.2K  │
│  Eng. Rate  ██  2.3%        │
└─────────────────────────────┘

📈 Performance: Above average for this time slot
📝 Suggestion: Threads tend to get 2x engagement — consider threading next time.
```

## Pitfalls

1. **Twitter API rate limits** — The X/Twitter API has strict rate limits. The `xurl` skill handles this, but batch posting across multiple drafts may hit limits. Space posts at least 5 minutes apart.

2. **Telegram bot permissions** — The Telegram bot must be an admin in the target channel to post messages. Verify bot permissions before scheduling.

3. **Character limits** — Twitter: 280 characters (or 4,000 for Blue/Premium users). Telegram: 4,096 characters. The skill warns on overflow but allows manual override.

4. **Media attachments** — Images must be under 5MB for Twitter and 10MB for Telegram. Video limits differ significantly across platforms.

5. **Timezone handling** — All times are interpreted as the user's local timezone. The cron scheduler and `/post-schedule` both use local time. Check with `/speed-read-config show` for timezone settings.

6. **Draft expiration** — Scheduled drafts older than 30 days are flagged for review. They are not auto-deleted but may reference outdated content.

7. **Hashtag research accuracy** — Hashtag popularity data is estimated based on recent trends. Numbers are approximate and vary by platform and time of day.

8. **Thread ordering** — When composing Twitter threads, posts must be published in sequence. The skill handles ordering but network errors can break threads.

9. **Concurrent posts** — Publishing the same draft to both platforms simultaneously can fail if one platform is slow. The skill publishes sequentially (Twitter first, then Telegram).

10. **Content calendar sync** — The Obsidian calendar is the source of truth. If you manually edit calendar files, ensure YAML frontmatter stays valid.

## Verification Steps

1. **Check skill is loaded:**
   ```
   /post-list all
   ```
   Should return an empty list (or existing posts) without errors.

2. **Test draft creation:**
   ```
   /post-draft twitter "Test post from social-poster skill" --tone professional
   ```
   Should return a draft ID and preview of the formatted post.

3. **Verify tone presets:**
   Create drafts with each tone (`professional`, `casual`, `hype`) and confirm the output style matches the description.

4. **Test hashtag research:**
   ```
   /hashtag-research testing
   ```
   Should return a list of relevant hashtags with popularity estimates.

5. **Test scheduling:**
   ```
   /post-schedule <draft-id> 2025-12-31T23:59
   ```
   Confirm the post appears in `/post-list scheduled` and the Obsidian calendar.

6. **Verify Obsidian integration:**
   Check `~/obsidian-vault/2-Projects/Social Media/Calendar/` for the calendar file and `2-Projects/Social Media/Posts/` for individual post notes.

7. **Test publishing (dry run):**
   Create a draft and publish it. Verify a post ID is returned and engagement tracking is available.

8. **Test cron setup:**
   ```
   /post-cron add "*/5 * * * *" telegram "Cron test — delete me"
   /post-cron list
   /post-cron remove <id>
   ```
   Confirm the cron job appears and can be removed cleanly.

9. **Test engagement tracking:**
   ```
   /engagement-track <post-id>
   ```
   Should display likes, retweets, replies, and impressions with a performance assessment.