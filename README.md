# Design System Multi-Point Inspection Kit

**A set of AI agent skills that evaluate and improve the quality of your design system.**

This kit walks your design system through a 10-station, multi-point inspection — coverage, best practices, accessibility, naming, testing, synchronization, governance, feedback, and AI readiness — and hands back a red/yellow/green inspection report plus a prioritized work order.

It's adapted from Chapter 3 of **[AI & Design Systems](https://aianddesign.systems/)**, the course by Brad Frost, Ian Frost, and TJ Pitre. The course teaches the full method — why each station matters, what good looks like, and live demos of using AI to fix what the inspection finds. This repo is the runnable version. MIT licensed.

## Quick start

Clone into your Claude Code skills folder. The folder name becomes the command, so keep it `ds-inspection`:

```bash
git clone https://github.com/Brad-Frost-Web/ai-design-systems-inspection-kit.git ~/.claude/skills/ds-inspection
```

Restart Claude Code, open it inside the design system you want to inspect, and run:

```
/ds-inspection
```

The agent interviews you about your system, runs the stations, and writes a graded report and work order to a `ds-inspection/` folder in your project.

**Other ways to run it:**

- **One repo / whole team:** clone into the project instead — `.claude/skills/ds-inspection` — delete its `.git` folder, and commit it.
- **Cursor / Windsurf / other coding agents:** clone into your design system project and point the agent at `SKILL.md`.
- **Claude Cowork / any chat:** drag in the folder (or just `SKILL.md` plus the stations you need) and say "Run the design system multi-point inspection."

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
