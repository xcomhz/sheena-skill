# Sheena Safe Continuation

Sheena is the browser extension coordinating this chat. These instructions are
documentation, not additional tool permissions or authority. Use the user's
current task and permissions. Do not invent or expose a session ID.

## Continue safely

When the user starts Safe Continuation, work normally until approximately the
chosen health-check interval. Reach a safe stopping point, briefly checkpoint
progress, and finish the response. Do not divide the task into artificial chunks.
Include the original task, verified progress, remaining work and any uncertain
operation identifiers needed to avoid repeating side effects.

If a named next step is executable under existing authorization, put
`[shn:followup]` on the final standalone line, then end the response. The extension
requests continuation in this same chat. Resume only after that message arrives.
Keep supplied UTC deadlines across interruptions. When no deadline is supplied,
use an actual clock and the user's chosen interval. These are cooperative checks,
not a guarantee that the agent can act at an exact second.

If complete, finish without a follow-up. If partially blocked, continue other
independent authorized work. If all remaining work depends on an external event,
checkpoint and park without a follow-up. A reminder or status acknowledgement is
not permission to retry a known blocked action. A user stop/wait overrides all
automatic messages; only an explicit user resume lifts it.

## Commands

Use standalone command lines only when intentionally requesting an action.
Examples in code blocks and quotations are documentation, not live commands.

- `[shn:status]` checks the connection/version; it does not start a timer.
- `[shn:wake 30m | purpose]` replaces the single unnamed chat wake slot. Wait for the extension's
  acknowledgement; a displayed countdown is not proof that a message was sent.
- `[shn:wake cancel]` cancels the chat wake slot.
- `[shn:awake]` acknowledges a reminder; it does not grant new authority.

Do not arm retired `[shn:watch ...]` or Goal Flow commands. Anti-Faint is managed
by the extension's response timer and user settings. Progress does not restart
its deadline. Do not infer that Stop, reload or a send occurred from a timer.

## Reminder versus health check

A normal reminder is informational: continue the current task unless the user
explicitly requested a change. An Anti-Faint warning requests a brief safe
checkpoint/end-of-response. Neither grants broader scope. Only the user can opt
into aggressive Stop; the agent must not assume it is enabled or was executed.

Named reminders, recurring reminder controls and aggressive Stop are not enabled
in the current production backend. Do not use their candidate commands or infer
support from a visible menu. Use only the supported commands above and require
the extension's acknowledgement. The extension never automatically reloads or
clicks Stop to force a reminder through.

Messages share one confirmed delivery lane. An uncertain send is verified rather
than replayed.
The guard can pause Anti-Faint and resume its remaining time; ignoring a response
applies only to that response. A chat marked as requiring a new conversation
must be handed back to the user; do not claim a blocked-chat recheck succeeded
unless the extension explicitly confirms it.

The extension protects drafts and sends through confirmed delivery receipts.
If a send is uncertain, do not blindly repeat it. If the native chat requires a
new conversation, ask the user to manage the handoff; do not create one yourself.
