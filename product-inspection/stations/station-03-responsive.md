---
station: 3
name: Responsive
quality: Adaptive (quality 3 of 10)
question: Does the product work across the breakpoints, input modes, and devices its audience actually uses — not just the width it was designed at?
---

# Station 3 — Responsive

**Quality: Adaptive.** Designs are drawn at a comfortable desktop width; users show up on phones, tablets, split-screen windows, zoomed-in browsers, and touchscreens. This station checks whether the product holds its shape and stays operable across that real range — reflow without horizontal scroll, tap targets big enough for thumbs, content legible at 200% zoom, no functionality that only works with a hover.

## Evidence to gather

| What to look at | Best source | Fallbacks |
|---|---|---|
| Breakpoint behavior | Live instance resized across mobile / tablet / desktop widths | Screenshots at each width · interview |
| Real-device check | Live instance on an actual phone (or device emulation) | Screenshots from a device · interview |
| Horizontal overflow | Rendered layout at narrow widths — any sideways scroll? | Screenshots · interview |
| Touch ergonomics | Tap-target sizes, hover-only affordances | Interaction on a touch device · interview |
| Zoom / reflow | Browser at 200% zoom; content reflows, nothing clipped | Screenshots · interview |
| Audience device mix | Analytics: actual viewport/device distribution | Interview: "what are your users on?" |

## Inspection procedure

1. **Establish the target range from real usage.** Pull the audience's actual device/viewport mix from analytics if reachable; otherwise ask. Score against *their* range, not a generic one.
2. **Walk the core flows at three widths** — a small phone (~360px), tablet (~768px), and desktop. Layout should reflow sensibly at each, not just shrink or clip.
3. **Hunt horizontal overflow** at narrow widths — the classic break. Any element forcing sideways scroll is a red flag; find the offender (fixed widths, unwrapped tables, oversized media).
4. **Touch ergonomics:** tap targets comfortably thumb-sized (~44px), adequate spacing, and — critically — **no action available only on hover** (menus, tooltips-as-content, reveal-on-hover controls) that a touch user can't trigger.
5. **Zoom / reflow to 200%:** text reflows into a single readable column, nothing is clipped or overlapped, and no content is lost. (This is also a WCAG requirement — cross-links to Station 2.)
6. **Check orientation and split-screen** if the audience uses them.

## Warning lights

- Horizontal scroll at common phone widths
- Fixed pixel widths, unwrapped tables, or media that won't shrink
- Tap targets too small or too close together
- Hover-only menus/controls/content with no touch equivalent
- Content clipped, overlapped, or lost at 200% zoom
- A layout that's clearly desktop-only for an audience that's mobile-heavy

## Scoring anchors

- **Red (0–3):** Core flows broken or unusable on the audience's common devices — overflow, clipped content, or touch users locked out of features.
- **Yellow (4–7):** Works across widths but with rough edges — a hover-only affordance, a table that overflows, tap targets a bit tight, one flow awkward on mobile.
- **Green (8–10):** The team can honestly say: *"Every core flow works and reads well across the devices our users actually bring, including touch and 200% zoom, with no horizontal overflow."*

## Turning off the light

- The offenders are usually a short list of fixed-width elements and unwrapped tables — an agent can find every one; wrapping/reflowing them is mechanical.
- Replace hover-only affordances with tap-and-focus equivalents (this doubles as an a11y fix).
- If reflow issues trace to layout components in the design system, file upstream — but most responsive drift is composition, which is the product's to fix.

## Station record

```markdown
### Station 3 — Responsive: <RED|YELLOW|GREEN> (<n>/10)
- Widths checked: <list> · Real device: <yes/no, which> · Audience mix: <from analytics/interview>
- Evidence level: <live / screenshot / interview, per check>
- Findings:
  - [verified|reported] <finding + evidence>
- Not inspected: <what and why>
- Deviations noted: <intentional departures>
- First move: <single highest-leverage fix>
```
