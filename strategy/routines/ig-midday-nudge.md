---
name: ig-midday-nudge
schedule: 12:00 PM EST daily
description: Send a midday Telegram nudge to comment on today's 5 IG picks before the algo push fades
---

Send a midday nudge message to the @planwithsama IG Picks Telegram group.

```
curl -s -X POST "https://api.telegram.org/bot[BOT_TOKEN]/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{"chat_id": -5190016930, "text": "⏰ Midday check — have you commented on today'\''s 5 picks yet?\n\nAlgo push on fresh posts fades by 3 PM EST. Still time if you go now.\n\nScroll up for your picks ☝️"}'
```

One curl command, then done. No tracker update needed.
