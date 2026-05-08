# Instagram Engagement System — @planwithsama

A daily IG engagement pipeline for solo founders. Telegram-native: URLs go in via the group, picks and nudges come back the same way. No email drafts, no inbox clutter.

**Daily time cost:** ~15 minutes (2 min URL grab → 10 min commenting → done)

**Automated:** URL collection, post visiting, follower-count lookup, bucket scoring, comment suggestion, reply detection, follow-conversion analysis.

**Manual:** Hashtag scrolling on IG mobile app (Instagram blocks web hashtag browsing); writing/editing comments; sending comments.

---

## Configuration

| Setting | Value |
|---|---|
| Handle | @planwithsama |
| Timezone | EST |
| Telegram group | IG Picks (chat ID: -5190016930) |
| Vibe accounts | @amandarachlee, @nabela, @thehomeedit |

**Hashtags scrolled daily:**
#planwithsama #mentalload #aiforwomen #womenproductivity #sundayreset #planwithme2026 #aiproductivity #workingmom #digitalplanner #weeklyplanning #womenwholead #secondshift #productivitytools #aiplanning #overwhelmedmom

---

## Your daily flow

### Morning (2 min on phone)
1. Open Instagram mobile, scroll your hashtags sorted by **Recent**
2. For each post you want to submit: tap Share → Copy Link → switch to **Telegram IG Picks group** → paste
3. Repeat for 6–15 posts. No compose, no subject line, no send button — just paste and scroll.

### 9:09 AM — Task runs automatically
Claude reads the group, visits each URL, scores against the 5 buckets, and posts **Today's 5** back into the same Telegram group with suggested comments.

### Midday (10 min on phone or laptop)
Open the Telegram group. For each of the 5 picks:
- Tap the link → Instagram opens → read the caption
- Use the suggested comment as-is, edit it, or write your own
- Comment → swipe back to Telegram → next pick

### 12:00 PM — Nudge arrives
If you haven't commented yet, a reminder lands in the group: *"Algo push fades by 3 PM."*

### 9:01 PM — Evening sweep runs automatically
Claude visits each post, finds your comment, scores it Strong/OK/Weak, checks for creator replies, posts a summary to the group. No signal needed from you.

### Sunday 8:01 AM — Weekly digest
Claude checks which accounts from the week's comments now follow @planwithsama, breaks down conversion by bucket and comment grade, posts the report to the group.

---

## The five buckets

| Bucket | Definition | Why |
|---|---|---|
| PEER | Other planner/productivity creators | Network-building. Their audience is yours. |
| DREAM CUSTOMER | 300–5,000 followers, posting about your customer's pain | Real human conversion candidates. |
| MICRO-INFLUENCER | 10K–50K, in-niche | Big enough to drive visits, small enough to reply. |
| FRESH POST | Posted in the last 24 hours | Algo is still pushing — comments have visibility. |
| AESTHETIC MATCH | Vibe matches brand (warm, intentional, founder energy) | Right eyeballs. |

Empty bucket slot > bad pick. Drop posts older than 4 weeks.

---

## The comment rule (this is what makes it work)

Every comment, two sentences max:
1. **React to one specific thing** they said in the caption — quote a phrase, reference a detail. Never a generic compliment.
2. **Ask a question** specific to their niche that they could answer in one line.

No pitch. No "love this!" No emoji floods. Sound like a real woman who actually read the caption.

**Grades:**
- **Strong ✅** — specific reaction + question
- **OK 🟡** — has a POV but generic or missing the question
- **Weak 🔴** — generic, no question, no specific reaction

---

## The four scheduled tasks

| Time | Task | What it does |
|---|---|---|
| 9:09 AM daily | `ig-morning-curate` | Reads Telegram group for URLs, scores, posts Today's 5 with suggested comments |
| 12:00 PM daily | `ig-midday-nudge` | Sends nudge if it's past noon |
| 9:01 PM daily | `ig-evening-sweep` | Visits posts, finds comments, scores them, posts summary |
| 8:01 AM Sunday | `ig-weekly-follow-check` | Profile follow checks, conversion by bucket, weekly digest |

All tasks: post to Telegram group only. Never send email. Never auto-comment.

Tasks require Chrome to be open with the Claude for Chrome extension active and Instagram logged in.

---

## Connector dependencies

1. **Telegram bot** — token stored in scheduled task config (keep private, do not commit)
2. **Telegram group** — IG Picks, chat ID: `-5190016930`
3. **Claude for Chrome extension** — installed, signed in, Chrome open during task hours
4. **Cowork app** — running for scheduled tasks to fire

---

## Pre-flight checklist (do this once)

Go to **Scheduled** in the Claude Code sidebar. Click **Run now** on each task in this order:
1. `ig-midday-nudge` — simplest, just sends a Telegram message. Approve the bash tool.
2. `ig-morning-curate` — approve bash + Chrome tools.
3. `ig-evening-sweep` — approve bash + Chrome tools.
4. `ig-weekly-follow-check` — approve bash + Chrome tools.

Running each once pre-approves the tool permissions so future scheduled runs don't pause waiting for you.

---

## Optimise for follows, not replies

Replies are a process metric. Follows are the asset. Sunday report breaks conversion down by bucket — cut what isn't converting, double down on what is.

---

*Last updated: 2026-05-07. Telegram-native rebuild — replaces the email-draft workflow.*
