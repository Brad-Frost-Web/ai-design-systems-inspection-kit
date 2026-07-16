---
station: 8
name: Visual craft & polish
quality: Polished (quality 8 of 10)
question: As rendered, do spacing, alignment, rhythm, and consistency read as considered and finished — or do the small imperfections add up to "unpolished"?
---

# Station 8 — Visual craft & polish

**Quality: Polished.** Users don't consciously notice good spacing; they absolutely feel bad spacing. This station inspects the rendered surface for craft — consistent rhythm, true alignment, deliberate hierarchy, even optical balance. With a solid design system, tokens should make much of this automatic, so findings here often reveal *where the product overrode or bypassed the system* (Station 1's drift showing up visually) or where composition — not the components — is off. It's distinct from fidelity (are the right pieces used?) and clarity (do the words work?): this is whether the assembled result looks finished.

## Evidence to gather

| What to look at | Best source | Fallbacks |
|---|---|---|
| Spacing rhythm | Rendered pages: consistent, token-scale spacing between/within blocks | Screenshots · interview |
| Alignment | Rendered: edges and baselines line up; grid honored | Screenshots (overlay a grid) · interview |
| Visual hierarchy | Rendered: type scale, weight, and emphasis guide the eye | Screenshots · interview |
| Consistency | Same patterns rendered the same way across pages | Side-by-side screenshots · interview |
| Optical details | Icon sizing/alignment, border/radius consistency, image crops | Zoomed screenshots · interview |
| Theme/dark-mode | If supported: both themes rendered and checked | Screenshots per theme |

## Inspection procedure

1. **Scan for spacing rhythm.** Is vertical and horizontal spacing on a consistent scale (ideally the system's spacing tokens), or arbitrary and uneven? Cramped here, floaty there is the tell.
2. **Check alignment.** Overlay a mental (or literal) grid: do element edges, text baselines, and columns actually line up? Look for the half-pixel-off, the one card that's indented, the label that doesn't share a baseline with its field.
3. **Read the hierarchy.** Squint at each page — does emphasis (size, weight, color, spacing) guide the eye to what matters, or is everything shouting/flat? Is the type scale coherent?
4. **Check cross-page consistency.** The same construct (a card, a section header, a toolbar) should render identically across pages. Divergence signals one-off overrides — cross-link to Station 1.
5. **Inspect optical details:** icons sized and aligned to their text, consistent border widths and radii, images cropped/positioned deliberately (no squished aspect ratios), consistent shadow/elevation.
6. **If the product themes** (light/dark, brand variants), render each and check both — polish often survives one theme and breaks in the other.

## Warning lights

- Inconsistent, off-scale spacing; cramped or floaty regions
- Misalignment — edges, baselines, or columns that don't line up
- Flat or confused hierarchy; incoherent type scale
- The same construct rendered differently on different pages
- Misaligned/mis-sized icons, inconsistent radii/borders, squished or badly cropped images
- Polish that holds in one theme and breaks in another

## Scoring anchors

- **Red (0–3):** Visibly unfinished — pervasive misalignment, arbitrary spacing, inconsistent rendering of the same patterns. Reads as a prototype.
- **Yellow (4–7):** Generally tidy with recurring small flaws — a few alignment misses, some spacing inconsistency, a construct that varies page to page.
- **Green (8–10):** The team can honestly say: *"Spacing and alignment are consistent and on-scale, hierarchy is clear, the same patterns render identically everywhere, and the details hold up under zoom and across themes."*

## Turning off the light

- Most polish issues trace back to app CSS overriding the system — fixing Station 1's drift usually fixes these for free (that's the leverage; sequence Station 1 first).
- Alignment/spacing offenders are findable by an agent diffing rendered spacing against the token scale; the fix is swapping arbitrary values for tokens.
- Cross-page inconsistency of a construct is the signal to extract it into a shared component/recipe.

## Station record

```markdown
### Station 8 — Visual craft & polish: <RED|YELLOW|GREEN> (<n>/10)
- Pages reviewed: <n> · Spacing/alignment: <summary> · Cross-page consistency: <summary> · Themes checked: <list>
- Evidence level: <live / screenshot / interview, per check>
- Findings:
  - [verified|reported] <finding + evidence>
- Not inspected: <what and why>
- Deviations noted: <intentional departures>
- First move: <single highest-leverage fix>
```
