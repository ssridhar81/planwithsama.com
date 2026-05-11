---
name: ig-weekly-follow-check
schedule: 8:01 AM EST Sunday
description: Check if this week's commented accounts now follow @planwithsama, analyse conversion by bucket, post digest to Telegram, update Excel tracker
---

You are running the weekly Instagram follow-conversion check for @planwithsama. Today is Sunday.

CONFIGURATION:
- Telegram bot token: [BOT_TOKEN]
- Telegram group chat ID: -5190016930

---

## STEP 1 — COLLECT THIS WEEK'S ENGAGEMENT DATA FROM TELEGRAM

```
curl -s "https://api.telegram.org/bot[BOT_TOKEN]/getUpdates?allowed_updates=[\"message\"]&limit=100"
```

Find all messages in chat_id -5190016930 from the past 7 days that start with "🌙 Evening Sweep". Extract:
- Each @handle engaged with
- Their bucket (PEER / DREAM CUSTOMER / MICRO-INFLUENCER / FRESH POST / AESTHETIC MATCH)
- Comment grade (Strong / OK / Weak / Not commented)

---

## STEP 2 — CHECK FOLLOWS VIA CHROME

For each handle:
1. Visit instagram.com/[handle] in Chrome
2. Look for "Follows you" indicator near bio
3. Record: yes / no / could not determine

---

## STEP 3 — ANALYSE CONVERSION

Calculate follow rate by bucket and by comment grade (Strong/OK/Weak). Identify best and worst performing buckets.

---

## STEP 4 — POST SUNDAY DIGEST TO TELEGRAM

```
curl -s -X POST "https://api.telegram.org/bot[BOT_TOKEN]/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": -5190016930, "parse_mode": "HTML", "disable_web_page_preview": true, "text": "[MESSAGE]"}'
```

Format:

📊 <b>Weekly Follow Report — w/e [date]</b>

<b>This week:</b> [N] comments · [N] replies · [N] new follows

<b>By bucket:</b>
PEER: [N] → [N] follows ([X]%)
DREAM CUSTOMER: [N] → [N] follows ([X]%)
MICRO-INFLUENCER: [N] → [N] follows ([X]%)
FRESH POST: [N] → [N] follows ([X]%)
AESTHETIC MATCH: [N] → [N] follows ([X]%)

<b>By comment grade:</b>
Strong ✅ → [X]% · OK 🟡 → [X]% · Weak 🔴 → [X]%

<b>Verdict:</b>
✅ Double down on: [best bucket]
⚠️ Review: [0% bucket if any]

New week starts now — paste your links when you're ready.

---

## STEP 5 — UPDATE EXCEL TRACKER (Followed Me? in Comment Log + Followed? in Today's Picks)

Run one command per handle:

```
python3 "C:\Users\shard\OneDrive\Documents\GitHub\planwithsama.com\tracker\update_tracker.py" follow --handle "@handle" --followed "yes"
```

Use `yes`, `no`, or `unknown`. This updates both the Comment Log (Followed Me? column) and Today's Picks (Followed? column) for any matching handle.
