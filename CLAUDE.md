# Shift Scheduler — CLAUDE.md

## Project Overview

A single-page web application (pure HTML/CSS/JS, no build step, no backend) that generates rotating shift schedules. Open the HTML file directly in a browser — all logic runs client-side.

## What the App Does

Generates a time-block schedule where exactly **2 people work per block** and the rest rest. The algorithm rotates pairs to maximize variety: everyone works with everyone else as evenly as possible.

## Inputs

| Input | Type | Details |
|---|---|---|
| Start time | Time picker | Start of the shift day |
| End time | Time picker | End of the shift day |
| Block duration | Select | 15, 20, 30, or 40 minutes |
| Participants | Text list | 4–8 names, one per line or comma-separated |

## Business Logic

1. **Block calculation** — compute total minutes between start and end, divide by block duration to get the exact number of blocks.
2. **Pair assignment** — each block gets exactly one pair (2 people). All others rest that block.
3. **Rotation algorithm** — distribute pairs across blocks so:
   - No pair repeats until all possible pairs have been used.
   - Work load is balanced: each person works roughly the same number of blocks.
   - Pair variety is maximized: everyone works with everyone else as many times as possible and as evenly as possible.

### Algorithm approach
- Pre-generate all possible pairs (n choose 2).
- Cycle through pairs in round-robin order across blocks.
- If blocks > number of pairs, repeat the full cycle; each repetition is a new round so distribution stays fair.

## Output

A schedule table:

| Time block | Person A | Person B |
|---|---|---|
| 08:00 – 08:30 | Ana | Carlos |
| 08:30 – 09:00 | Beatriz | Diego |
| … | … | … |

Optionally highlight each person's blocks with a consistent color per participant.

## File Structure

```
sheduler/
  index.html   ← single file: markup + CSS + JS all inline
```

No frameworks, no npm, no build tooling. Must work by opening `index.html` directly (`file://` protocol).

## UI / UX Notes

- Clean, minimal interface.
- Form at the top, generated schedule below.
- "Generate" button triggers the calculation.
- Schedule should be printable (consider a basic print stylesheet).
- Mobile-friendly layout.
