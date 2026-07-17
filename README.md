# Design System Multi-Point Inspection Kit

**A set of AI agent skills that evaluate and improve the quality of your design system.**

This kit walks your design system through a 10-station, multi-point inspection as detailed in the **[AI & Design Systems](https://aianddesign.systems/)**, the course by Brad Frost, Ian Frost, and TJ Pitre. The inspection hands back a graded inspection report plus a prioritized work order for addressing your design system's check engine light.

The kit includes **two companion inspections** that share the same method: **`ds-inspection`** puts your *design system* on the lift, and **`product-inspection`** inspects a *product built with it* — as real users meet it. They're designed to run in tandem: inspect the system, then inspect what got shipped on top of it. Most of this README covers `ds-inspection`; see **[Product inspection](#product-inspection)** for the product one.

## Quick start

1. In a new terminal window, navigate to your design system directory. 
2. Run the following command, which uses the [skills CLI](https://github.com/vercel-labs/skills): 

```bash
npx skills add Brad-Frost-Web/ai-design-systems-inspection-kit
```

It detects which agents you use and installs the kit's inspection skills (`ds-inspection` and `product-inspection`) where each one looks for them. Or install at the user-level, so all projects can access the skills:


```bash
npx skills add Brad-Frost-Web/ai-design-systems-inspection-kit -g
```
3. Then open your agent inside the design system, then say "Run the design system multi-point inspection" or run the slash command:

```
/ds-inspection
```

To inspect a **product** built with your system instead, run `/product-inspection` from inside that product's project — see [Product inspection](#product-inspection) below.

Or run a single station like so:

```
/ds-inspection run station 1
```

4. The agent then interviews you about your system, runs the stations, and writes a graded report and work order to a `ds-inspection/` folder in your project.



**Other ways to install and run:**

- **Manual install (no npx):** clone this repo and copy the inner `ds-inspection/` folder into your agent's skills directory — e.g. `~/.claude/skills/ds-inspection` for Claude Code:
  ```bash
  git clone https://github.com/Brad-Frost-Web/ai-design-systems-inspection-kit.git
  cp -R ai-design-systems-inspection-kit/ds-inspection ~/.claude/skills/ds-inspection
  ```
  The folder name becomes the command, so keep it `ds-inspection`.
- **Whole team:** run `npx skills add` in the project (without `-g`) and commit the installed skill so everyone who works in that repo gets it.
- **Claude Cowork / any chat:** drag in the `ds-inspection` folder (or just `SKILL.md` plus the stations you need) and say "Run the design system multi-point inspection."

## What it checks

The 10 stations cover the five qualities of a healthy, AI-ready design system:

| Quality | Question | Stations |
|---|---|---|
| **1. Complete** | Does your system have what products need? | 1. Coverage & gaps |
| **2. Sound** | Is what's in the system actually good? | 2. Best practices · 3. Accessibility · 4. Shared language · 5. Testing & validation |
| **3. Synchronized** | Are assets connected & orchestrated? | 6. Orchestration |
| **4. Extensible** | Can you reliably improve, extend, and evolve the system? | 7. Governance & version control · 8. Feedback & adoption |
| **5. AI-Ready** | Can AI successfully use the design system? | 9. Machine-readable docs & context · 10. Agent access |

## Tool-agnostic by design

This kit is plain markdown. There is no required tool, plugin, or vendor. Every station gathers evidence through a fallback chain:

1. **Live tool access** — if your agent can reach your design file (any Figma MCP — Figma's official server, Figma Console MCP, or another bridge), your repo, or your docs site, it inspects directly.
2. **Exports** — no live access? Paste or attach exports: token JSON, variable exports, a component inventory, package metadata.
3. **Screenshots** — a picture of your Figma layers panel or docs page is real evidence.
4. **Interview** — no artifacts at all? The station asks you the questions and records your answers.

Every finding is tagged **[verified]** (the agent saw it directly) or **[reported]** (you described it). Both count; only one is proof. A report full of `[reported]` findings is your cue to get the agent better access next time.

**Optional power-up:** connect [design-systems-mcp](https://github.com/southleft/design-systems-mcp) by Southleft — a free, hosted knowledge server covering Material, Fluent, Carbon, Polaris, Spectrum, Primer, Lightning, and Atlassian, plus DTCG and WCAG standards. Station 1 uses it to benchmark your component coverage against the industry (it's how we found the popover, menu, tree-view, and loading-state gaps in our own system); other stations use it for standards lookups. No install — it's a remote endpoint:

```bash
claude mcp add --transport http design-systems https://design-systems-mcp.southleft.com/mcp
```

(Other agents: add `https://design-systems-mcp.southleft.com/mcp` as a remote MCP server in your agent's config.) Without it, the agent benchmarks from its own knowledge and says so.

## Your design system is more than this repo

A design system spans more than the codebase you run this in: a Figma library, a documentation site, reference websites, and the products that consume it all. The kit handles this through **intake**: on first run, the agent interviews you about what makes up your system and tests what it can actually reach, then records it all in `ds-inspection/GARAGE.md`.

`GARAGE.md` is plain markdown and it's yours to edit. Add Figma file URLs, docs and reference site links, product repo paths, support channels — anything the inspection should consider. Every station reads it before gathering evidence, and the more of it your agent can actually reach (via MCP servers, file access, or fetchable URLs), the more findings come back `[verified]` instead of `[reported]`. Commit it with your project and future runs skip the interview; update it whenever your system changes.

## The three ways to run it

| Mode | Say | What happens |
|---|---|---|
| **Full inspection** | "Run the full multi-point inspection" | Intake (if needed) → all 10 stations → inspection report + work order |
| **Single station** | "Run station 3" / "Inspect our accessibility" | One station, one station record appended to your latest report |
| **Re-inspection** | "Re-inspect" / "Run station 4 again" | Same checks, compared against your previous report — what changed, what didn't |

## What it leaves behind

The inspection writes to a `ds-inspection/` folder in your project:

```
ds-inspection/
├── GARAGE.md                     ← your system's profile ("the vehicle on the lift")
├── reports/
│   └── 2026-07-08-inspection.md  ← dated inspection reports (red/yellow/green, /100 score)
└── work-orders/
    └── 2026-07-08-work-order.md  ← prioritized fixes: reds first, yellows scheduled
```

Keep these in version control. The dated reports are how you see drift over time — the whole point is a cadence, not a one-time event. Deep inspection quarterly; wire the everyday checks (stations 1–6) into CI so problems surface the moment they appear.

## The score is a conversation starter, not a grade

Each station scores 0–10 (red 0–3, yellow 4–7, green 8–10), summing to a /100. Fix the reds first, schedule the yellows, count your greens, and re-run on a cadence. Scale the frame to your team — a two-person system and a 200-person platform hit different bars for green.

## Product inspection

A design system exists to build products with. `product-inspection` is the companion skill that puts one of those **products** on the lift — a shipping app, website, or feature — and inspects it **as real users meet it**, not as designed or as coded. Same method as the design-system inspection: same `[verified]`/`[reported]` evidence chain, the same three run modes, the same 0–10 → /100 scoring — pointed at a shipped product.

Run it from inside the product's project (after installing the kit):

```
/product-inspection
```

Or say "Run the product multi-point inspection." Single station and re-inspection work the same way — `/product-inspection run station 2`, "inspect this product's accessibility," "re-inspect." It writes to a `product-inspection/` folder, a sibling of `ds-inspection/`, so both inspections coexist in one project.

**Get the product running first.** Unlike the design-system inspection, this one is about the product *as shipped* — so six of its ten stations need a **reachable running instance** (a staging URL or a local dev server the agent can load and drive) to return `[verified]` findings instead of `[reported]` guesses. The intake probes for one before anything else.

The 10 stations:

| # | Station | The question it answers |
|---|---|---|
| 1 | Design system adoption | Is the product actually consuming the system — correctly, as shipped — or has it drifted into one-off CSS and detached components? |
| 2 | Best practices | The Lighthouse-style catch-all: responsive across devices, deliberate loading/empty/error/success states, graceful failure under stress, clean web-platform hygiene. |
| 3 | Accessibility | On the real running DOM: keyboard, focus, ARIA, contrast, semantics — does it hold up where users are? |
| 4 | Content & information architecture | Labels, navigation, microcopy, findability — can people understand and get around? |
| 5 | Usability | Can a real user complete the product's core jobs end to end, without getting stuck or dropped? |
| 6 | Visual design | Beyond tidy spacing: a cohesive design language — hierarchy, balance, brand distinctness — or merely assembled parts? |
| 7 | Performance | Load, latency, and payload weight on a real connection and device. |
| 8 | Security & privacy | Auth, sessions, permissions, data exposure, dependency vulnerabilities, and input handling as-shipped. |
| 9 | Testing & validation | A real suite covering the core flows, running in CI as a gate (and the inspection's own automatable checks wired in), so quality holds as the product changes. |
| 10 | Measurement & feedback | Can the team see how the product performs in the wild, and does that signal feed back into the backlog and up into the design system? |

The inspection leans on proven open tooling as reference instruments — [Lighthouse CLI](https://github.com/GoogleChrome/lighthouse) for performance, axe-core/pa11y for accessibility, `npm audit` for dependencies — as exemplars, never hard requirements. Judgment-based stations share [NN/g's ten usability heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) as a common vocabulary. And intake gathers the team's **stated intent** (decks, goals, brand references) so the inspection scores the shipped product against the product's *own* ideal, not just generic bars.

Where a product symptom traces to the components themselves rather than how they were composed, that's a **design-system** finding — run `ds-inspection` and cross-reference. That's the tandem.

## Other skills worth knowing

There are other great skills out there for evaluating the health of your design system and code, like [Design System Ops](https://designsystemops.com/) by Murphy Trueman, [Google's Modern Web Guidance](https://developer.chrome.com/docs/modern-web-guidance), and others. Several station files point to Design System Ops skills as good follow-up tools for the problems the inspection surfaces.

## AI & Design Systems
These skills are helpful, but we'd love it if you joined our [AI & Design Systems](https://aianddesign.systems/) course and community to watch detailed walk-throughs, learn core concepts for wielding AI & Design Systems together, and join a vibrant community of AI & Design Systems practitioners. 

MIT licensed.
