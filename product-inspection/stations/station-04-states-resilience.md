---
station: 4
name: States & resilience
quality: Resilient (quality 4 of 10)
question: Does the product handle everything that isn't the happy path — loading, empty, error, and success states deliberately designed, and graceful behavior under bad input, failed requests, offline, and edge-case data?
---

# Station 4 — States & resilience

**Quality: Resilient.** The happy path is the easy 20%. Real products live in the other states — the spinner while data loads, the empty list on first use, the inline error when a request fails — and real conditions are hostile: flaky networks, malformed input, enormous datasets, expired sessions, double-clicked submit buttons. This station inspects both faces of the same coin: are the non-happy-path states *designed* (not defaulted-to-nothing), and does the product *behave* well when pushed — failing safely, preserving the user's work, offering a way back? Missing states are where products feel broken; brittle failure handling is where trust is lost. (NN/g lens: this station is "visibility of system status," "error prevention," and "help users recognize, diagnose, and recover from errors" made concrete.)

## Evidence to gather

| What to look at | Best source | Fallbacks |
|---|---|---|
| Loading states | Live instance: throttle network, watch each async surface | Screen recording · interview |
| Empty states | Live instance: view lists/collections with zero items (fresh account) | Screenshots · interview |
| Error states & failure surfacing | Live instance: force failures (offline, bad input, 500); how errors reach the user vs. console/logs | Screenshots · console · interview |
| Success / in-progress / disabled | Live instance: complete actions, watch button/control states and confirmations | Screen recording · interview |
| Input validation | Live: bad/empty/oversized/malformed input into forms | Repo validation code · interview |
| Network failure | Live: offline mid-flow, dropped requests, slow/timeout | Screen recording · interview |
| Boundary data | Live: zero items, huge lists, long strings, special chars | Screenshots · interview |
| Concurrency | Live: double-click submit, rapid repeated actions | Interview |
| Code-level coverage | Repo: do components branch on loading/empty/error, or assume data? | Interview |

## Inspection procedure

**Part A — are the states designed?**

1. **List the core async surfaces** — every place the product fetches, submits, or waits. For each, the four states to verify: **loading, empty, error, success.** Build the inventory as a grid (surface × state) — the holes are immediately visible.
2. **Loading:** throttle the network (devtools) and load each surface. Is there a deliberate loading treatment (skeleton/spinner/progress), or a blank flash / layout jump / frozen UI?
3. **Empty:** reach each collection with zero items (a fresh account or filtered-to-nothing). Is there a designed empty state that orients and offers a next step — or a bare blank region that reads as broken?
4. **Success & in-progress:** complete actions. Is there confirmation? During submission, is the control disabled/loading to prevent double-submit? Is optimistic UI reconciled if the request fails?

**Part B — does it hold up under stress?**

5. **Attack the inputs.** Into each core form: empty required fields, wrong types, absurdly long strings, special characters/emoji, pasted junk. Is it validated (ideally before submit) with clear guidance — or does it break, submit garbage, or silently fail?
6. **Break the network.** Go offline mid-flow; throttle to timeout. Does the product detect it, tell the user, **preserve entered data**, and let them retry — or hang, blank out, or lose the form?
7. **Push boundary data.** Zero items and hundreds; very long names/strings; very large/small numbers. Look for overflow, layout breakage, performance cliffs, truncation that loses meaning.
8. **Provoke concurrency:** double-click submit; fire the same action rapidly. Double-submits, duplicate records, race conditions?
9. **Watch how failures surface.** Errors should reach the *user* as actionable messages — not only the console, and never as a raw stack trace or leaked internal detail. Cross-check in code where live triggering is hard: do components actually branch on these states?

## Warning lights

- Blank screens or layout jumps standing in for a loading state
- Empty collections that look like errors (no designed empty state)
- Failures that are silent, show raw errors/stack traces, or hang with no feedback
- No confirmation after a consequential action
- Buttons that stay enabled during submit (double-submit risk); no in-progress state
- Input that isn't validated — breaks, submits garbage, or fails silently
- Network failures that lose the user's entered data
- Boundary data that overflows, breaks layout, or tanks performance
- Optimistic updates that don't roll back on failure

## Scoring anchors

- **Red (0–3):** Core surfaces have only the happy path — failures silent or destructive, loading blank, empties look broken, work lost under ordinary failure conditions. Users can't tell working from stuck, and mistakes are unrecoverable.
- **Yellow (4–7):** Most surfaces handle the common states, but gaps remain — a silent failure here, a missing empty state there, an unvalidated input, work lost on a dropped request, a boundary case that breaks layout.
- **Green (8–10):** The team can honestly say: *"Every core surface deliberately handles loading, empty, error, and success — and bad input, failed requests, offline, and edge-case data all fail gracefully: the product preserves work, tells the user something actionable, and offers a way to recover."*

## Turning off the light

- The state-inventory grid (surface × loading/empty/error/success) *is* the punch list — empty and error states are often the highest-value, lowest-effort wins; a good empty state doubles as onboarding.
- Client-side validation with clear inline guidance prevents a whole class of failures; an agent can add it systematically.
- Preserving form state across failures (don't clear the form on a failed submit) is small and disproportionately trust-building.
- Add double-submit guards (disable-on-submit, idempotency) where actions mutate data.
- Route user-facing errors through a consistent handler so nothing leaks raw.
- If the design system ships state patterns (skeleton, empty-state, alert/toast components), wiring them in is fast; if it lacks them, that's a DS gap to file upstream.

## Station record

```markdown
### Station 4 — States & resilience: <RED|YELLOW|GREEN> (<n>/10)
- Surfaces inventoried: <n> · State grid coverage: <loading/empty/error/success hit-rate> · Stress applied: <inputs / network / boundary / concurrency> · Worst failure: <what happened>
- Evidence level: <live / recording / code / interview, per check>
- Findings:
  - [verified|reported] <finding + evidence>
- Not inspected: <what and why>
- Deviations noted: <intentional departures>
- First move: <single highest-leverage fix>
```
