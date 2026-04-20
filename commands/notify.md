---
description: Send a Telegram notification summarizing the work just completed
allowed-tools: Bash(curl:*), Bash(tmux:*), Bash(basename:*), Bash(hostname:*), Bash(date:*), Bash(pwd:*), Bash(source:*)
argument-hint: [optional focus or extra note]
---

Send a Telegram notification to summarize the task you just completed in this session.

## Steps

1. Write a concise summary (2-5 short lines, markdown allowed) of what was accomplished. Focus on:
   - What was the goal
   - Key changes/results (files touched, experiments run, conclusions)
   - Any follow-ups the user should know about
   
   If the user provided extra context in `$ARGUMENTS`, weight your summary toward that focus.

2. Send the notification by running this bash command (fill in `<SUMMARY>` with your summary text, keep it under ~500 chars, escape backticks and special chars for shell):

```bash
source ~/.config/claude-notify.env && \
PROJECT=$(basename "$(pwd)") && \
TMUX_SESSION=$(tmux display-message -p '#S' 2>/dev/null || echo "no-tmux") && \
HOST=$(hostname -s) && \
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S') && \
MESSAGE="✅ *Claude Code Task Done*
🖥 \`${HOST}\` · 📁 \`${PROJECT}\` · 🪟 \`${TMUX_SESSION}\`
🕐 ${TIMESTAMP}

<SUMMARY>" && \
curl -s -X POST "https://api.telegram.org/bot${TG_BOT_TOKEN}/sendMessage" \
  -d "chat_id=${TG_CHAT_ID}" \
  -d "parse_mode=Markdown" \
  --data-urlencode "text=${MESSAGE}" > /dev/null && \
echo "Notification sent."
```

3. Confirm to the user that the notification was sent (one short line).

## Extra context from user

$ARGUMENTS
