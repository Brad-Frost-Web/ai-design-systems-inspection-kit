# Recording run-of-show — `product-inspection` on bradfrost.com

A one-pager for demoing the product inspection live. bradfrost.com is an Eleventy static site, so the "get it running" step is easy and the six as-shipped stations light up cleanly.

## Before you hit record (~5 min)

- [ ] **Get the site running.** `npm start` (or point the agent at the live URL). This is ground rule 4 — the whole inspection is "as shipped," so a reachable instance is what turns findings from `[reported]` into `[verified]`.
- [ ] **Open the repo** in the same session so the agent has code access too (adoption, testing, and security stations read source).
- [ ] **Install the skill:** `npx skills add Brad-Frost-Web/ai-design-systems-inspection-kit` — this adds `product-inspection` alongside the `ds-inspection` you already have.
- [ ] **Optional intent prep:** either jot a two-line "Intent & ideal" (pitch + core jobs + design ideal) so Stations 5/6 score against a real bar — or leave it blank and demo the skill's honest "no stated ideal, recorded as a gap" behavior on camera. Both are good TV.

## The arc (the story)

1. **Frame the tandem.** `ds-inspection` puts the *system* on the lift; `product-inspection` inspects *what shipped on top of it*. The station sets deliberately rhyme (Coverage↔Adoption, Best practices↔Best practices, Testing↔Testing, Feedback↔Measurement & feedback).
2. **Check in.** Show the intake / `GARAGE.md` access map — "here's what I can actually reach." Make the running-instance probe explicit.
3. **Run stations 1 → 10.** Narrate a few live rather than all ten:
   - **2 · Best practices** — resize to a phone width, watch for overflow; peek at the console. This is the Lighthouse-style catch-all (responsive + states + hygiene).
   - **3 · Accessibility** — axe-core against real routes → hard `[verified]` numbers.
   - **9 · Testing & validation** *(new)* — "does CI actually gate, or do good tests just run on someone's laptop?"
   - **10 · Measurement & feedback** *(reframed)* — the loop in both directions, including whether findings feed *up* into the design system.
4. **The report.** `/100`, red/yellow/green — say the line: *"a conversation starter, not a grade."*
5. **The work order.** Reds first. Point at the single highest-leverage fix.
6. **(If time — the money shot.)** Do ONE fix live, then **re-inspect** and watch the light turn green. That fix→re-inspect loop is the most convincing 90 seconds you can record.

## Talking points to land

- **Evidence discipline.** Every finding is `[verified]` or `[reported]`; nothing is guessed. (On Asset Factory the skill even caught and *retired its own false positive* — a "gap" that turned out to be a color-picker overlay artifact. Great honesty beat if it comes up.)
- **Grounded, not invented.** Lighthouse categories + NN/g's usability heuristics + it rhymes with `ds-inspection`. Not a bespoke rubric.
- **The score isn't the point — the work order is.** You finish knowing exactly what to do Monday.

## Gotchas / what to avoid on camera

- **Static Eleventy site = smooth.** No DB/services/auth to wrangle (unlike the Nuxt/web-component apps the skill was built against).
- **Web-component apps get finicky** through a browser bridge — synthetic clicks don't always fire framework handlers, and a native `<input type=color>` can pop an OS picker that blocks the renderer. Not a concern for bradfrost.com, but if you ever demo on an Eddie app, lean on axe injection + API-level checks.
- **Performance (Station 7):** dev-build numbers lie. Either run a production build first or just narrate the `[reported]` caveat — the skill is explicit about it.
- **Nothing mutates.** The inspection reads and reports; it doesn't change the site.

## Prebaked artifacts you can reference

Both live in each project's `product-inspection/` folder — good "here's one we did earlier" cutaways:

- **Asset Factory** — full baseline → work order → real fixes → re-inspection, score **63 → 68**, three lights turned green.
- **Content Brain** — **56/100** under the final structure; the new stations earned their keep (surfaced a 234-test suite with *no CI*, SSR hydration console errors, and a genuinely working upstream-to-Eddie feedback loop).

---

_Run with `/product-inspection` from inside the product's project. Writes to a `product-inspection/` folder — a sibling of `ds-inspection/`._
