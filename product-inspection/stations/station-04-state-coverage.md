---
station: 4
name: Interaction & state coverage
quality: Complete (quality 4 of 10)
question: Are loading, empty, error, and success states designed and handled across the product — or does only the happy path exist?
---

# Station 4 — Interaction & state coverage

**Quality: Complete.** The happy path is the easy 20%. Real products live in the other states: the spinner while data loads, the empty list on first use, the inline error when a request fails, the disabled button mid-submit, the confirmation after success. Missing states are where products feel broken, unfinished, or untrustworthy — a blank screen that's actually "loading," a form that silently swallows a failure. This station inventories the states of each core surface and checks that each is deliberately designed, not defaulted-to-nothing.

## Evidence to gather

| What to look at | Best source | Fallbacks |
|---|---|---|
| Loading states | Live instance: throttle network, watch each async surface | Screen recording · interview |
| Empty states | Live instance: view lists/collections with zero items (fresh account) | Screenshots · interview |
| Error states | Live instance: force failures (offline, bad input, 500) | Screenshots · interview |
| Success/confirmation | Live instance: complete actions, watch for feedback | Screen recording · interview |
| In-progress / disabled | Live instance: submit forms, watch button/control states | Screenshots · interview |
| Code-level coverage | Repo: do components branch on loading/empty/error, or assume data? | Interview |

## Inspection procedure

1. **List the core async surfaces** — every place the product fetches, submits, or waits. For each, the four states to verify: **loading, empty, error, success.**
2. **Loading:** throttle the network (devtools) and load each surface. Is there a deliberate loading treatment (skeleton/spinner/progress), or a blank flash / layout jump / frozen UI?
3. **Empty:** reach each collection with zero items (a fresh account or filtered-to-nothing). Is there a designed empty state that orients and offers a next step — or a bare blank region that reads as broken?
4. **Error:** force failures — go offline, submit invalid input, trigger a server error if you can. Does the product catch it and tell the user something actionable, or fail silently / show a raw stack / hang?
5. **Success & in-progress:** complete actions. Is there confirmation? During submission, is the control disabled/loading to prevent double-submit? Is optimistic UI reconciled if the request fails?
6. **Cross-check in code** where live triggering is hard: do the components actually branch on these states, or render assuming data is present?

## Warning lights

- Blank screens or layout jumps standing in for a loading state
- Empty collections that look like errors (no designed empty state)
- Failures that are silent, show raw errors, or hang with no feedback
- No confirmation after a consequential action
- Buttons that stay enabled during submit (double-submit risk); no in-progress state
- Optimistic updates that don't roll back on failure

## Scoring anchors

- **Red (0–3):** Core surfaces have only the happy path — failures silent, loading blank, empties look broken. Users can't tell working from stuck.
- **Yellow (4–7):** Most surfaces handle the common states, but gaps remain — a silent failure here, a missing empty state there, inconsistent loading treatments.
- **Green (8–10):** The team can honestly say: *"Every core surface deliberately handles loading, empty, error, and success — the product always tells the user what's happening."*

## Turning off the light

- Build the state inventory as a grid (surface × loading/empty/error/success) — the holes are immediately visible and become the punch list.
- Empty and error states are often the highest-value, lowest-effort wins — a good empty state doubles as onboarding.
- If the design system ships state patterns (skeleton, empty-state, alert/toast components), wiring them in is fast; if it lacks them, that's a DS gap to file upstream.

## Station record

```markdown
### Station 4 — Interaction & state coverage: <RED|YELLOW|GREEN> (<n>/10)
- Surfaces inventoried: <n> · State grid coverage: <loading/empty/error/success hit-rate>
- Evidence level: <live / recording / code / interview, per check>
- Findings:
  - [verified|reported] <finding + evidence>
- Not inspected: <what and why>
- Deviations noted: <intentional departures>
- First move: <single highest-leverage fix>
```
