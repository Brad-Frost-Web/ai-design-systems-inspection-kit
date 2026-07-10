# Design System Multi-Point Inspection Kit

**A set of AI agent skills that evaluate and improve the quality of your design system.**

This kit walks your design system through a 10-station, multi-point inspection as detailed in the **[AI & Design Systems](https://aianddesign.systems/)**, the course by Brad Frost, Ian Frost, and TJ Pitre. The inspection hands back a graded inspection report plus a prioritized work order for addressing your design system's check engine light.

## Quick start

Install with the [skills CLI](https://github.com/vercel-labs/skills), which works across agents — Claude Code, Cursor, Codex, Copilot, Windsurf, Gemini CLI, and more. From your design system project:

```bash
npx skills add Brad-Frost-Web/ai-design-systems-inspection-kit
```

It detects which agents you use and installs the `ds-inspection` skill where each one looks for it. Add `-g` to install user-level (all projects) instead of just this project.

Then open your agent inside the design system you want to inspect and run:

```
/ds-inspection
```

(Or say "Run the design system multi-point inspection" — any agent that reads the skill can follow it.) The agent interviews you about your system, runs the stations, and writes a graded report and work order to a `ds-inspection/` folder in your project.

**Don't want the full inspection?** Run a single station:

```
/ds-inspection run station 1
```

Any station works this way — `/ds-inspection run station 3`, or plain language like "inspect our accessibility." The agent asks only the intake questions that station needs, runs it, and writes that station's record to your report.

**Other ways to install and run:**

- **Manual install (no npx):** clone this repo and copy the inner `ds-inspection/` folder into your agent's skills directory — e.g. `~/.claude/skills/ds-inspection` for Claude Code:
  ```bash
  git clone https://github.com/Brad-Frost-Web/ai-design-systems-inspection-kit.git
  cp -R ai-design-systems-inspection-kit/ds-inspection ~/.claude/skills/ds-inspection
  ```
  The folder name becomes the command, so keep it `ds-inspection`.
- **Whole team:** run `npx skills add` in the project (without `-g`) and commit the installed skill so everyone who works in that repo gets it.
- **Claude Cowork / any chat:** drag in the `ds-inspection` folder (or just `SKILL.md` plus the stations you need) and say "Run the design system multi-point inspection."

> **Installed before the npx layout (July 2026)?** The skill files moved into a `ds-inspection/` subfolder of this repo, so a `git pull` inside an old clone at `~/.claude/skills/ds-inspection` will break it. Delete that folder and reinstall with either method above.

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

## Other skills worth knowing

There are other great skills out there for evaluating the health of your design system and code, like [Design System Ops](https://designsystemops.com/) by Murphy Trueman, [Google's Modern Web Guidance](https://developer.chrome.com/docs/modern-web-guidance), and others. Several station files point to Design System Ops skills as good follow-up tools for the problems the inspection surfaces.

## AI & Design Systems
These skills are helpful, but we'd love it if you joined our [AI & Design Systems](https://aianddesign.systems/) course and community to watch detailed walk-throughs, learn core concepts for wielding AI & Design Systems together, and join a vibrant community of AI & Design Systems practitioners. 

MIT licensed.
