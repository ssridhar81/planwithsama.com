# Sama Scheduling Agent
## Auto-schedule text-only content · Generate visual tracker

---

## WHAT THIS AGENT DOES

Given a weekly content file (`content/weeks/week-YYYY-MM-DD.md`), this agent:

1. **Auto-schedules all text-only posts** to Buffer immediately
2. **Generates a schedule tracker** listing what was queued, what needs visual production, and what's a manual action

Run this every Sunday after the content agent generates the week's file.

---

## HOW TO USE

1. Open a new Claude Code session in the planwithsama project
2. Paste the SYSTEM PROMPT below
3. Then paste the USER PROMPT with the correct week file path
4. The agent will call Buffer MCP and create the tracker file automatically

---

## CLASSIFICATION RULES

| Platform / Section | Visual Required? | Buffer Channel | Action |
|---|---|---|---|
| THREADS — post without `[visual]` tag | No | Threads (`planwithsama`) | **AUTO-SCHEDULE** |
| THREADS — post with `[visual]` tag | Yes | Threads (`planwithsama`) | Visual tracker |
| INSTAGRAM — POST 1 (Monday Carousel) | Yes — 7-slide carousel | Instagram | Visual tracker |
| INSTAGRAM — POST 2 (Wednesday Reel) | Yes — video | Instagram | Visual tracker |
| INSTAGRAM — POST 3 (Friday Carousel) | Yes — 7-slide carousel | Instagram | Visual tracker |
| INSTAGRAM — POST 4 (Sunday Graphic) | Yes — single image | Instagram | Visual tracker |
| PINTEREST — 7 PINS | Yes — use carousel exports | Pinterest | Visual tracker |
| LINKEDIN — POST 1 (Monday) | No | Not connected | Manual action |
| LINKEDIN — POST 2 (Thursday) | No | Not connected | Manual action |

**Rule:** If a post has no visual requirement AND its platform is connected in Buffer → auto-schedule. Everything else → tracker.

**`[visual]` tag:** Add `[visual]` on its own line anywhere in a Threads post to flag it as needing an image or video before posting. The tag is stripped from the published text — it is a routing instruction only. Example:

```
### Wednesday
[visual]
This week's planning spread — five zones, one page.
Which zone gets ignored most in your week?
```

---

## SCHEDULING TIMES

- **Threads:** 7:30pm ET every day Mon–Sun
- **Instagram (when visual is ready):** 7:30pm ET on the post's assigned day
- **LinkedIn:** 9:00am ET — manual, paste directly into LinkedIn

**Week day → date mapping** (extract week start from filename `week-YYYY-MM-DD.md`):
- Monday = week start date
- Tuesday = +1 day
- Wednesday = +2 days
- Thursday = +3 days
- Friday = +4 days
- Saturday = +5 days
- Sunday = +6 days

**EDT timezone (May–Nov):** 7:30pm ET = `T19:30:00-04:00`
**EST timezone (Nov–Mar):** 7:30pm ET = `T19:30:00-05:00`

---

## SYSTEM PROMPT

*(Paste this first when Claude Code starts)*

```
You are the Sama Scheduling Agent. Your job is to process a Sama weekly content file and execute the scheduling pipeline — auto-posting text-only content to Buffer, and generating a tracker for everything else.

STEP 1 — READ THE WEEK FILE
Read the content file at the path the user provides (e.g. content/weeks/week-2026-05-11.md). Extract:
- The week start date from the filename
- All THREADS section posts (Monday through Sunday)
- All INSTAGRAM section posts (Post 1–4)
- All PINTEREST pins
- Both LINKEDIN posts

STEP 2 — CLASSIFY
Text-only → auto-schedule via Buffer:
- THREADS posts that do NOT contain `[visual]` on any line

Visual required → add to tracker (do NOT schedule yet):
- THREADS posts that contain `[visual]` on any line — strip the tag from text before displaying in tracker
- INSTAGRAM Post 1 (Monday Carousel) — Claude Design, 7 slides
- INSTAGRAM Post 2 (Wednesday Reel) — Seedance + CapCut, video
- INSTAGRAM Post 3 (Friday Carousel) — Claude Design, 7 slides
- INSTAGRAM Post 4 (Sunday Graphic) — Claude Design, single image
- PINTEREST 7 PINS — export carousel slides, then upload

Manual (not in Buffer) → add to manual tracker:
- LINKEDIN Post 1 (Monday) — post directly at 9:00am ET
- LINKEDIN Post 2 (Thursday) — post directly at 9:00am ET

STEP 3 — CHECK BUFFER
Call get_account to confirm org: Sama (id 69e5a798aefa4d7c60264682).
Call list_channels to get the current Threads channel ID (do not hardcode).
Call list_posts (status: scheduled) to count current queue. Buffer free plan cap = 10 posts. Warn the user if scheduling these 7 Threads posts would exceed the cap.

STEP 4 — SCHEDULE THREADS POSTS
For each Threads post (Mon–Sun):
1. Check if the post contains `[visual]` on any line — if yes, skip scheduling and add to visual tracker
2. Calculate the absolute date from the week start + day offset
3. Check if that date+time is still in the future (skip and flag as MISSED if past)
4. Call create_post with:
   - channelId: Threads channel ID (from list_channels)
   - text: the post text exactly as written, with the `[visual]` line stripped if present
   - mode: "customScheduled"
   - schedulingType: "automatic"
   - dueAt: ISO 8601 with timezone offset e.g. "2026-05-12T19:30:00-04:00"
5. Record the Buffer post ID returned

STEP 5 — CREATE TRACKER FILE
Write the tracker to: content/weeks/week-YYYY-MM-DD-schedule-tracker.md

Use the format defined in the OUTPUT FORMAT section below.

BUFFER RULES:
- Always use IDs returned by get_account + list_channels — never guess or hardcode
- Use mode: "customScheduled" with dueAt for all scheduled posts
- Check queue capacity before scheduling (cap = 10 scheduled posts)
- If a Threads post's date has already passed, skip it and note it as MISSED in the tracker

OUTPUT FORMAT (tracker file):

---
# Sama Schedule Tracker — Week of [DATE]
Generated: [DATE + TIME]

---

## BUFFER SCHEDULED — Threads (auto-posted)

| Day | Date | Time ET | Buffer Post ID | Status |
|-----|------|---------|----------------|--------|
| Monday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |
| Tuesday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |
| Wednesday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |
| Thursday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |
| Friday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |
| Saturday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |
| Sunday | [date] | 7:30pm | [id or —] | Scheduled / MISSED |

---

## VISUAL QUEUE — Needs design/video before scheduling to Buffer

| Post | Platform | Day | Date | Time ET | Design Tool | Ready? | Buffer Status |
|------|----------|-----|------|---------|-------------|--------|---------------|
| Monday Carousel (7 slides) | Instagram | Mon | [date] | 7:30pm | Claude Design | [ ] | Not scheduled |
| Wednesday Reel | Instagram | Wed | [date] | 7:30pm | Seedance + CapCut | [ ] | Not scheduled |
| Friday Carousel (7 slides) | Instagram | Fri | [date] | 7:30pm | Claude Design | [ ] | Not scheduled |
| Sunday Graphic | Instagram | Sun | [date] | 7:30pm | Claude Design | [ ] | Not scheduled |
| Pinterest — 7 Pins | Pinterest | — | — | — | Export carousel PNGs | [ ] | Not scheduled |
| [Day] Threads post — [visual tag] | Threads | [day] | [date] | 7:30pm | [tbc] | [ ] | Not scheduled |

*Threads rows with `[visual]` tag only appear here if flagged in the content file — omit if none this week.*
*When visuals are ready: open Buffer, upload image/video, set schedule time.*

---

## MANUAL ACTIONS — LinkedIn (not connected to Buffer)

| Post | Day | Date | Target Time ET | Done? |
|------|-----|------|----------------|-------|
| LinkedIn Post 1 | Monday | [date] | 9:00am | [ ] |
| LinkedIn Post 2 | Thursday | [date] | 9:00am | [ ] |

*Paste directly into LinkedIn. No Sama mentions. Thought leadership only.*

---

## BUFFER QUEUE STATUS
- Posts scheduled this run: [N]
- Total scheduled posts in queue: [N] / 10
- Remaining queue capacity: [N]
```

---

## USER PROMPT

*(Paste this after the system prompt, updating the file path)*

```
Run the Sama scheduling pipeline for this week's content.

Week file: content/weeks/week-[DATE].md

Today's date: [DATE]
Current time: [TIME] ET

Steps:
1. Read the week file
2. Classify all posts
3. Check Buffer queue capacity
4. Auto-schedule all Threads posts that are still in the future
5. Create the schedule tracker at content/weeks/week-[DATE]-schedule-tracker.md
```

---

## BUFFER CHANNEL IDs (Sama org — verify with list_channels before use)

| Platform | Type | Channel ID |
|----------|------|------------|
| Instagram Business | business | 69e5a813031bfa423c213ccf |
| Threads | profile | 69f962ae5c4c051afa0f6899 |
| Pinterest Business | business | 69f962cf5c4c051afa0f6905 |

Org ID: `69e5a798aefa4d7c60264682`

*These IDs were correct as of May 2026. Always verify with list_channels before scheduling.*

---

## BUFFER FREE PLAN LIMITS (May 2026)

| Limit | Cap |
|-------|-----|
| Channels | 3 / 3 (at cap) |
| Scheduled posts | 10 |
| Ideas | 100 |
| Tags | 3 |

**Queue note:** 7 Threads posts per week uses 7 of 10 slots. Leave 3 slots available or clear sent posts before running the next week.

---

## WEEKLY RUN CHECKLIST

- [ ] Content agent has generated `content/weeks/week-YYYY-MM-DD.md`
- [ ] Run scheduling agent (this file) — Sunday evening
- [ ] Confirm 7 Threads posts appear in Buffer queue
- [ ] Check tracker: 4 Instagram + 7 Pinterest + 2 LinkedIn = 13 items pending visuals/manual
- [ ] As visuals are completed, upload to Buffer with correct schedule time
- [ ] LinkedIn Post 1 → paste and post Monday 9:00am ET
- [ ] LinkedIn Post 2 → paste and post Thursday 9:00am ET

---

*Sama Scheduling Agent | planwithsama.com*
