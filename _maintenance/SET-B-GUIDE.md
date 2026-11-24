# Maintenance Kit — Mock Paper Set B (Paper 2B & Paper 4B)

This folder is **not published** to the live site (GitHub Pages ignores `_`-prefixed
folders). It exists so that adding Set B later is a copy-and-adjust job with zero
risk to students' saved progress.

| File | Purpose |
|---|---|
| `SET-B-GUIDE.md` | The exact recipe for adding Paper 2B / Paper 4B |
| `dfm-skill-index.json` | Parsed Dr Frost skill sheet: 928 skills + 3,477 letter-level subskills (code → description). For picking DFM labels on new questions. |
| `dfm-mapping-set-a.json` | Every DFM code assigned to Set A questions, pairs and diagnostics — the reference for format and coverage. |

The site was pre-wired on 24 Nov 2026 so that Set B slots in without touching
existing behaviour:

- Dashboard has two **Set B** cards (Paper 2B, Paper 4B). Both are live.
  Each card switches independently when its flag
  (`MOCK_SET_B2_AVAILABLE` / `MOCK_SET_B4_AVAILABLE`) is set to `true`
  in `index.html`; score status and "Copy Weak Spots" then track that paper
  automatically.
- `defaultState()` in `index.html` already contains `paper2b` / `paper4b`
  placeholders, and every page's migration hook (`ID_MIGRATIONS.mockNums`)
  already knows the `paper2b` / `paper4b` state keys.

---

## SET-B-GUIDE — step by step

### 1. Create the files

Copy `mock-paper2.html` → `mock-paper2b.html` and `mock-paper4.html` → `mock-paper4b.html`, then in each new file find & replace **all occurrences**:

| Find (in the 2B copy) | Replace with |
|---|---|
| `state.mocks.paper2` | `state.mocks.paper2b` |
| `'_p2clock'` | `'_p2bclock'` |
| `PAPER2_QUESTIONS` | `PAPER2B_QUESTIONS` |
| `<title>Mock Paper 2 (Non-Calculator)` | `<title>Mock Paper 2B (Non-Calculator)` |
| `PAPER 2 · NON-CALC` | `PAPER 2B · NON-CALC` |
| `href="mock-paper4.html"` (header nav) | `href="mock-paper4b.html"` + label `Paper 4B →` |
| `Open Paper 2 Mock →` (bottom bar) | `Open Paper 2B Mock →` |
| `End of Paper 2 (75 Marks)` | `End of Paper 2B (75 Marks)` |

(And the mirror-image pairs for the 4B copy: `paper4`→`paper4b`,
`'_p4clock'`→`'_p4bclock'`, `PAPER4_QUESTIONS`→`PAPER4B_QUESTIONS`, etc.)

Also update the header count line (`16 Questions · 75 Marks`) and bottom-bar
total if your Set B papers have a different number of questions.

### 2. Replace the questions

Swap in new questions keeping the existing object shape:
`{ num, strand, marks, dfm, prompt, solution }`.
`num` may repeat Set A's numbering (1, 2, 3 …) — marks are stored under
`state.mocks.paper2b.perQuestion`, a separate namespace, so nothing collides.

**Calculator rules (site policy, already checked for Set A):**
- Paper 2B: every answer must be exact / mental-friendly — fractions, surds,
  integers, clean decimals. Verify each by hand or script before publishing.
- Paper 4B: calculator allowed; answers at 3 s.f. / 2 d.p. / 1 d.p. as normal.

### 3. DFM labels

1. Pick granular letter codes for each new question from
   `dfm-skill-index.json` (top-level keys `skillTitles` and `subskills`).
2. Put `dfm: '<code>, <code>'` on each question object (after `marks:`).
3. Replace the `DFM_INFO` map near the top of the file with a new map holding
   only the codes used in that paper (code → description). The pill renderer
   (`dfmPills`) already ships in the copied file.

`dfm-mapping-set-a.json` shows exactly how Set A was labelled.

### 4. Switch the dashboard on

In `index.html`:

```js
const MOCK_SET_B2_AVAILABLE = true;   // set true when mock-paper2b.html exists
const MOCK_SET_B4_AVAILABLE = true;   // set true when mock-paper4b.html exists
```

That is the **only** index.html change needed. Each card becomes a live link
independently, status text starts tracking its scores, and the revision summary
includes that paper's B score.

Optionally also update the "What Should I Do Next?" recommender to suggest a
Set B re-sit after both Set A papers are complete.

### 5. Test before pushing

- Open each B page you added: timer, +25% toggle, mark-scheme lock, submit,
  per-strand breakdown, DFM pills, calculator banner.
- Refresh mid-timer: clock resumes (the copied exam-clock code just works with
  the new `_p2bclock` key).
- Confirm Set A pages and student scores are untouched (they must be — Set A
  files are not edited at all).

### Progress-safety rules

1. **Never renumber or rename Set A data.** Set B lives entirely in separate
   state keys (`mocks.paper2b`, `mocks.paper4b`) and clock keys, so it cannot
   clash with anything students have already saved.
2. If Set B questions ever need renumbering *after* students have used them,
   add entries to `ID_MIGRATIONS.mockNums.paper2b` (or `.paper4b`) in the
   affected pages — the migration hook already iterates these keys.
3. Keep `total: 75` (or update the dashboard card copy to match).
4. Pushes to `master` auto-deploy in ~1 minute; avoid pushing mid-way through
   an exam window.
