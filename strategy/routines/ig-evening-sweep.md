---
name: ig-evening-sweep
schedule: 9:01 PM EST daily
description: Check today's 5 IG picks for comments left by @planwithsama, score each Strong/OK/Weak, post summary to Telegram, update Excel tracker
---

You are running the evening Instagram engagement sweep for @planwithsama.

CONFIGURATION:
- Telegram bot token: [BOT_TOKEN]
- Telegram group chat ID: -5190016930

---

## STEP 1 — FIND TODAY'S PICKS FROM TELEGRAM

```
curl -s "https://api.telegram.org/bot[BOT_TOKEN]/getUpdates?allowed_updates=[\"message\"]&limit=100"
```

Find the message sent today in chat_id -5190016930 that starts with "📋 Today's 5". Extract the 5 post URLs, handles, buckets, and suggested comments from that message. If no picks message found, send skip notice and stop:

```
curl -s -X POST "https://api.telegram.org/bot[BOT_TOKEN]/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": -5190016930, "text": "🌙 Evening sweep skipped — no picks message found for today."}'
```

---

## STEP 2 — CHECK COMMENTS VIA CHROME

For each of the 5 post URLs:
1. Open in Chrome
2. Find @planwithsama's comment — record exact text
3. Check if creator replied (look for a nested reply)

Note "not commented yet" if no comment found.

---

## STEP 3 — SCORE EACH COMMENT

- STRONG ✅ — References something specific from caption AND ends with a question
- OK 🟡 — Has a POV but generic or missing the question
- WEAK 🔴 — Generic compliment, no specific reaction, no question
- NOT COMMENTED ⬜ — No comment found

---

## STEP 4 — POST SWEEP SUMMARY TO TELEGRAM

```
curl -s -X POST "https://api.telegram.org/bot[BOT_TOKEN]/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": -5190016930, "parse_mode": "HTML", "disable_web_page_preview": true, "text": "[MESSAGE]"}'
```

Format:

🌙 <b>Evening Sweep — [date]</b>

@[handle] · [STRONG ✅ / OK 🟡 / WEAK 🔴 / NOT COMMENTED ⬜]
💬 "[actual comment, truncated to 20 words]"
[↩️ Creator replied | — No reply yet]

[repeat for all 5]

---
Strong: [N] · OK: [N] · Weak: [N] · Not commented: [N]
📊 Follow check runs Sunday 8 AM.

---

## STEP 5 — UPDATE EXCEL TRACKER (Comment Log + Today's Picks back-fill)

```
python3 "C:\Users\shard\OneDrive\Documents\GitHub\planwithsama.com\tracker\update_tracker.py" log --data "[LOG_JSON]"
```

Fields per pick: date (YYYY-MM-DD), handle, bucket, url, suggested_comment (from morning's Today's 5 message), your_comment (actual comment found), verdict (Strong/OK/Weak/Not commented), replied (yes/no).

This single call writes to Comment Log AND back-fills Your Comment, Verdict, Replied? into Today's Picks automatically.
