---
station: 9
name: Resilience & error handling
quality: Resilient (quality 9 of 10)
question: When things go wrong — bad input, failed requests, offline, edge-case data — does the product degrade gracefully and recover, or break, hang, and lose the user's work?
---

# Station 9 — Resilience & error handling

**Quality: Resilient.** Station 4 checks that error *states exist*; this checks how the product *behaves under stress* — the adversarial cousin. Real conditions are hostile: flaky networks, malformed input, empty or enormous datasets, expired sessions, race conditions from double-clicks. A resilient product anticipates these, fails safely, preserves the user's work, and offers a way back. A brittle one corrupts data, hangs, or dumps a stack trace. This is where trust is won or lost.

## Evidence to gather

| What to look at | Best source | Fallbacks |
|---|---|---|
| Input validation | Live: bad/empty/oversized/malformed input into forms | Repo validation code · interview |
| Network failure | Live: offline, dropped requests, slow/timeout | Screen recording · interview |
| Boundary data | Live: zero items, huge lists, long strings, special chars | Screenshots · interview |
| Session/auth edges | Live: expired session, unauthorized action | Interview |
| Concurrency | Live: double-click submit, rapid repeated actions | Interview |
| Failure surfacing | How errors reach the user vs. the console/logs | Console + rendered UI |

## Inspection procedure

1. **Attack the inputs.** Into each core form: empty required fields, wrong types, absurdly long strings, special characters/emoji, paste junk. Is it validated (ideally before submit), with clear guidance — or does it break, submit garbage, or silently fail?
2. **Break the network.** Go offline mid-flow; throttle to timeout. Does the product detect it, tell the user, preserve entered data, and let them retry — or hang, blank out, or lose the form?
3. **Push boundary data.** View a list with zero items and with hundreds; render very long names/strings; check very large or very small numbers. Look for overflow, layout breakage, performance cliffs, truncation that loses meaning.
4. **Test session/permission edges** if applicable: what happens when a session expires mid-action, or a user attempts something unauthorized? Graceful redirect/message, or a crash/blank?
5. **Provoke concurrency:** double-click submit; fire the same action rapidly. Double-submits, duplicate records, race conditions?
6. **Watch how failures surface.** Errors should reach the *user* as actionable messages — not only the console, and never as a raw stack trace or leaked internal detail.

## Warning lights

- Input that isn't validated — breaks, submits garbage, or fails silently
- Network failures that hang, blank the screen, or lose the user's entered data
- Boundary data that overflows, breaks layout, truncates meaning, or tanks performance
- Session/permission edges that crash or dump the user with no recovery
- Double-submits / race conditions creating duplicates or corruption
- Raw stack traces, console-only errors, or leaked internals shown to users

## Scoring anchors

- **Red (0–3):** The product breaks, hangs, corrupts data, or loses work under ordinary failure conditions; errors leak raw internals.
- **Yellow (4–7):** Handles the common cases but has brittle spots — an unvalidated input, work lost on a dropped request, a boundary case that breaks layout.
- **Green (8–10):** The team can honestly say: *"Bad input, failed requests, offline, and edge-case data all fail gracefully — the product preserves work, tells the user something actionable, and offers a way to recover."*

## Turning off the light

- Client-side validation with clear inline guidance is the highest-leverage fix and prevents a whole class of failures; an agent can add it systematically.
- Preserving form state across failures (don't clear the form on a failed submit) is small and disproportionately trust-building.
- Add double-submit guards (disable-on-submit, idempotency) where actions mutate data — cross-links to Station 4.
- Route user-facing errors through a consistent handler so nothing leaks raw.

## Station record

```markdown
### Station 9 — Resilience & error handling: <RED|YELLOW|GREEN> (<n>/10)
- Stress applied: <inputs / network / boundary / session / concurrency> · Worst failure: <what happened>
- Evidence level: <live / recording / code / interview, per check>
- Findings:
  - [verified|reported] <finding + evidence>
- Not inspected: <what and why>
- Deviations noted: <intentional departures>
- First move: <single highest-leverage fix>
```
