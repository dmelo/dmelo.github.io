---
title: "Claude Code: Handling the error \"API Error: The response stopped arriving\""
date: 2026-10-28
permalink: /blog/26/claude-code-api-error-response-stopped-arriving.html
---

# Claude Code: Handling the error `API Error: The response stopped arriving. The response above may be incomplete.`

## The problem

Every once in a while, I see a yellow warning on my claude code session:

```
API Error: The response stopped arriving. The response above may be incomplete.
```

Searching about this error, it is exactly as it says: the server has stopped sending data mid-response, for over a certain time. I have multiple sessions opened, all the time. What would usually happen:

1. I send a prompt
2. I move to other session 
3. get back the the initial prompt, hoping to see the reply, but find this warning instead.

This can be very frustating.

## How often it happens

When I ask Claude to look back at my sessions and measure how often I have seen this warning, it's clear that its happening a lot:

| Day        | Sessions hit | % of sessions | Errors | Errors per 1k API calls |
|------------|-------------:|--------------:|-------:|------------------------:|
| 2026-08-28 |       1 / 19 |          5.3% |      1 |                     0.5 |
| 2026-09-01 |        1 / 8 |         12.5% |      5 |                     2.4 |
| 2026-09-09 |       1 / 13 |          7.7% |      2 |                     0.9 |
| 2026-10-08 |        1 / 8 |         12.5% |      1 |                     2.8 |
| 2026-10-09 |        1 / 6 |         16.7% |      2 |                     3.7 |
| 2026-10-14 |       1 / 28 |          3.6% |      2 |                     0.4 |
| 2026-10-15 |       4 / 18 |         22.2% |     25 |                     7.2 |
| 2026-10-16 |       4 / 13 |         30.8% |      9 |                     5.9 |
| 2026-10-19 |       3 / 14 |         21.4% |      9 |                    12.5 |
| 2026-10-20 |        2 / 7 |         28.6% |      3 |                     5.8 |
| 2026-10-21 |        4 / 7 |         57.1% |      7 |                    13.5 |
| 2026-10-22 |       4 / 10 |         40.0% |      9 |                    26.5 |
| 2026-10-23 |       4 / 18 |         22.2% |      4 |                     6.3 |
| 2026-10-26 |       6 / 13 |         46.2% |     31 |                    17.1 |
| 2026-10-27 |        1 / 6 |         16.7% |     24 |                    40.3 |
| **All days, 2026-06-08 → 2026-10-28** | **38 / 1,483 session-days** | **2.6%** | **134** | **1.0** |

The rows above are only the days with at least one error. The last row covers every day in the period, error-free days included, which is why its percentage is much lower.

A note on the dates: this exact message first appeared on 2026-08-28, in Claude Code v2.1.238. Before that, the same failure was logged as "Connection closed mid-response" (45 times between June 19 and August 5), and a third wording, "Connection lost mid-response", shows up 23 more times from August 25 on. The table counts only the current wording, so it starts in late August because the message was renamed, not because the problem started then.

## The fix: a StopFailure hook

Given the frequency it's happening, finding a way to mitigate the effect was long overdue. What we (Claude and I) came up with was a hook that would nudge Claude Code after such error.

The hook used is `StopFailure`. I tested it on Claude Code v2.1.312; older versions may not have this hook, so run `claude --version` first. This is how to add this hook on your system. Replace `YOU` with yout actual username (assuming you're using MacOS/Linux). Add this to your `~/.claude/settings.json`:

```
{
  "hooks": {
    "StopFailure": [
      {
        "matcher": "server_error",
        "hooks": [
          {
            "type": "command",
            "command": "/Users/YOU/.claude/hooks/stream-resume.sh",
            "timeout": 180,
            "asyncRewake": true
          }
        ]
      }
    ]
  }
}
```

Key points of this hook:
1. StopFailure + server_error: runs only when a turn ends on an API failure. Rate limits, auth errors and overloads are skipped.
1. asyncRewake: true: the hook runs in the background. If it exits with code 2, its stderr is sent to the model as a new turn, so the session picks up without you typing "continue".
1. timeout: 180: leaves room for the longest backoff, which is 45 s.

After that, save the script as ~/.claude/hooks/stream-resume.sh, then run chmod +x ~/.claude/hooks/stream-resume.sh:

```
#!/usr/bin/env bash
# StopFailure hook: when a turn dies with server_error, wait, then wake the model with a resume note.
max=${STREAM_RESUME_MAX:-3}          # wakes allowed...
window=${STREAM_RESUME_WINDOW:-900}  # ...per this many seconds, per session
base=${STREAM_RESUME_BACKOFF:-15}    # backoff: 15s, 30s, 45s
state_dir="${XDG_CACHE_HOME:-$HOME/.cache}/claude-stream-resume"
mkdir -p "$state_dir"

in=$(cat)
read -r sid err <<<"$(python3 -c 'import json,sys; d=json.loads(sys.stdin.read() or "{}"); print(d.get("session_id","unknown"), d.get("error",""))' <<<"$in")"
[ "$err" = "server_error" ] || exit 0

f="$state_dir/$sid"
now=$(date +%s)
recent=$(awk -v t=$((now - window)) '$1 >= t' "$f" 2>/dev/null)
n=$(printf '%s' "$recent" | grep -c . )
printf '%s\n' "$recent" | grep . > "$f.tmp"; mv "$f.tmp" "$f"
printf '%s %s\n' "$(date '+%F %T')" "session=$sid stream failure, $n wake(s) already in the last ${window}s (cap $max)" >> "$state_dir/log"
if [ "$n" -ge "$max" ]; then
  echo "$(date '+%F %T') session=$sid cap reached, not waking" >> "$state_dir/log"
  exit 0
fi
echo "$now" >> "$f"
sleep $((base * (n + 1)))
echo "The API connection dropped mid-response (automatic resume $((n + 1)) of $max). Resume from where you stopped: don't repeat what you already wrote or did. If a tool call was in flight, check its result or state before running it again." >&2
exit 2
```

## What it looks like

After everything is in place, when you get a new `API Error: The response stopped arriving. The response above may be incomplete.`, it'll continue automatically. you might see something like this:

```
● API Error: The response stopped arriving. The response above may be incomplete.

✻ Cooked for 10m 44s · done 4:34 PM

● Stop hook feedback

● Checking whether the queue update went through, then I'll report CS-838 to you.
```

The stream dies (line 1) and the turn ends (line 2). Fifteen seconds later the hook exits with code 2; Claude Code prints its default "Stop hook feedback" label (line 3) and feeds the hook's note to the model, which resumes by checking whether its last action landed before going on (line 4).

To confirm the hook is working, check its log with `tail ~/.cache/claude-stream-resume/log`. Every failure gets a line with how many wakes that session already had, and a "cap reached, not waking" line appears when a session hits the limit of 3 wakes in 15 minutes.

This is a mitigation, not a fix. I'm confident Anthropic will release a proper fix very soon ;)
