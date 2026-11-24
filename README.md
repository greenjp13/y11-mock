# Y11 Mock — CIE 0580 Extended Revision Hub

Static revision site for Year 11 IGCSE Maths (CIE 0580 Extended, 2025–2027 syllabus).

**Live:** https://greenjp13.github.io/y11-mock/

## Contents

| Page | Purpose |
|---|---|
| `index.html` | Dashboard: RAG strand ratings, micro-skill checklists, 2-min diagnostics, formula sheet, readiness stats, mock launchpad |
| `strand-*.html` (7) | Decision trees & examiner traps, I Do / You Do pairs, exam practice banks with CIE-style mark schemes |
| `mock-paper2.html` / `mock-paper4.html` | 90-minute timed mocks (75 marks) with +25% extra time, mark-scheme lock, self-marking, strand breakdown |

All student progress is stored **in the browser** (localStorage key
`hub_0580_y11mock_2026_v1`), with JSON export/import for moving devices.

## Update safety

- Every question carries a granular Dr Frost Maths skill code (`dfm:` field),
  shown as a pill with the skill description on hover.
- An `ID_MIGRATIONS` hook in every page remaps students' saved progress if IDs
  are ever renamed — fill the maps rather than renaming in place.
- The mock exam clock persists across reloads/closed tabs (exam time keeps
  running; reload auto-resumes).
- Calculator rules are labelled explicitly everywhere: Paper 2 items are
  non-calculator, Paper 4 items allow a scientific calculator.

## Planned: Mock Set B

Paper 2B and Paper 4B (fresh parallel papers for second sittings) are marked
"Coming soon" on the dashboard. When the files are ready, follow
`_maintenance/SET-B-GUIDE.md` and flip `MOCK_SET_B_AVAILABLE` in `index.html`.
The `_maintenance/` folder holds the DFM skill index and Set A label mapping;
it is **not** published to the site (GitHub Pages skips `_` folders).

## Deploying

Push to `master` — GitHub Pages rebuilds in about a minute. Avoid pushes
mid-way through a scheduled mock window.
