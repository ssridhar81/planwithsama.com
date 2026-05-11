---
name: ig-morning-curate
schedule: 9:09 AM EST daily
description: Read Instagram URLs from Telegram group, score against 5 buckets, post top 5 picks with suggested comments back to group
---

You are running the daily Instagram engagement morning curation for @planwithsama.

CONFIGURATION:
- Telegram bot token: [BOT_TOKEN]
- Telegram group chat ID: -5190016930
- Telegram API base: https://api.telegram.org/bot[BOT_TOKEN]

NICHE CONTEXT for @planwithsama:
Hashtags: #planwithsama #mentalload #aiforwomen #womenproductivity #sundayreset #planwithme2026 #aiproductivity #workingmom #digitalplanner #weeklyplanning #womenwholead #secondshift #productivitytools #aiplanning #overwhelmedmom
Vibe reference accounts: @amandarachlee, @nabela, @thehomeedit

---

## STEP 1 — COLLECT URLs FROM TELEGRAM

Run this bash command to fetch today's messages from the group:
```
curl -s "https://api.telegram.org/bot[BOT_TOKEN]/getUpdates?allowed_updates=[\"message\"]&limit=100"
```

From the JSON response:
- Filter for messages where `chat.id` is `-5190016930`
- Find the most recent message in chat_id -5190016930 that starts with "📋 Today's 5" and note its `date` Unix timestamp — this is when the last run processed URLs.
- Collect all instagram.com URLs from messages sent AFTER that timestamp.
- If no "📋 Today's 5" message exists (first ever run), collect all instagram.com URLs from the last 7 days.
- Extract all unique instagram.com URLs from the `text` field of those messages

If fewer than 3 unique Instagram URLs are found, send this Telegram message and stop:
```
curl -s -X POST "https://api.telegram.org/bot[BOT_TOKEN]/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": -5190016930, "text": "📭 Not enough picks today — only [N] URLs found since 6 AM. Paste more Instagram links in this group and I will pick up where I left off at 9 AM tomorrow. Or paste links now and ask me to run manually."}'
```

---

## STEP 2 — VISIT EACH URL IN INSTAGRAM

Use Chrome to visit each Instagram URL. Instagram should already be logged in.

For each post, capture:
- Creator @handle
- Full caption text (or as much as visible)
- Post date/time (look for the timestamp)
- Post type (photo / reel / carousel)

Then navigate to the creator's profile page (click their handle) and capture:
- Follower count

Skip any post that is older than 4 weeks — do not include in scoring.

---

## STEP 3 — SCORE AGAINST 5 BUCKETS

Assign each post to its single best matching bucket. Select up to 5 picks, one per bucket. An empty bucket slot is better than a bad pick.

Bucket definitions:
1. PEER — Other planner, productivity, or organisation creators. Their audience overlaps with @planwithsama's target customer.
2. DREAM CUSTOMER — 300–5,000 followers. Posting about mental load, overwhelm, planning struggles, working mom stress, second shift, or AI tools for life. A real human conversion candidate.
3. MICRO-INFLUENCER — 10K–50K followers. In-niche (productivity, planning, working moms, AI tools for women). Big enough to drive profile visits, small enough to actually reply to comments.
4. FRESH POST — Posted within the last 24 hours. The algorithm is still actively pushing it — comments left now have maximum visibility.
5. AESTHETIC MATCH — Warm, intentional, founder energy. Visual and tonal vibe matches @amandarachlee, @nabela, @thehomeedit. Right eyeballs even if smaller account.

---

## STEP 4 — GENERATE SUGGESTED COMMENTS

For each of the 5 selected picks, write one suggested comment following this rule exactly:
- Sentence 1: React to ONE specific thing from the caption. Quote a phrase or name a specific detail — never a generic compliment.
- Sentence 2: Ask a question that is specific to their niche and situation, something they could realistically answer in one short line.
- No pitch. No "love this!" No emoji floods. Two sentences maximum. Sound like a real woman who actually read the caption carefully.

---

## STEP 5 — POST PICKS TO TELEGRAM GROUP

```
curl -s -X POST "https://api.telegram.org/bot[BOT_TOKEN]/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": -5190016930, "parse_mode": "HTML", "disable_web_page_preview": true, "text": "[MESSAGE]"}'
```

Format:

📋 <b>Today's 5 — [Weekday, Month Date]</b>

1️⃣ <b>[BUCKET NAME]</b> | @[handle]
👥 [follower count] · [post type] · [time ago]
🔗 [full post URL]
💬 "[first 15–20 words of caption]..."
✏️ <i>[suggested comment]</i>

[repeat for all 5]
[If a bucket has no suitable pick: ⚠️ <b>[BUCKET]</b> — no pick today]

---
⏰ Go comment — algo push is strongest before noon EST.

---

## STEP 6 — UPDATE EXCEL TRACKER (Today's Picks tab)

```
python3 "C:\Users\shard\OneDrive\Documents\GitHub\planwithsama.com\tracker\update_tracker.py" picks --data "[PICKS_JSON]"
```

Fields per pick: date (YYYY-MM-DD), bucket, handle, followers, url, caption_snippet, why, suggested_comment.
