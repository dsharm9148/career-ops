# Mode: scout — Scan → Filter → Evaluate (Full Discovery Pipeline)

Chains `scan` + smart pre-filter + `auto-pipeline` in one command. Discovers new offers, skips seniority mismatches, and evaluates everything that passes — without any manual steps.

---

## Step 0 — Sync Check

```bash
node cv-sync-check.mjs
```

Parse the output. If any errors are reported → warn the user and **abort**. Do not proceed with stale CV or profile data.

---

## Step 1 — Scan Portals

```bash
node scan.mjs
```

Parse stdout to extract the count of new offers found. Look for lines like `Found N new offers` or `Added N entries`.

- If **0 new offers** → report: "No new offers. All portals up to date." and stop.
- If **N > 0** → continue to Step 2.

---

## Step 2 — Read Pending Entries

Read `data/pipeline.md`. Collect all lines matching `- [ ]`.

Expected format per line:
```
- [ ] {url} | {company} | {title}
```

Lines without `|` separators (bare URLs) are also valid — treat company and title as unknown and keep them for evaluation.

---

## Step 3 — Smart Pre-Filter (Zero Tokens)

For each pending entry, check the **title field only** (no JD fetch — zero LLM cost at this stage).

**SKIP** the entry if the title contains any of the following (case-insensitive):

| Pattern | Reason |
|---------|--------|
| `VP`, `Vice President` | Executive level |
| `Director` | Director-level |
| `C-Level`, `CEO`, `CTO`, `CPO` | C-suite |
| `Principal` | Implies 8+ yrs |
| `Staff` | Implies 6+ yrs |
| `Senior` | Implies 4+ yrs, above new-grad |
| `Head of` | Leadership |
| `^Lead ` (Lead as first word) | e.g., "Lead ML Engineer" |

**KEEP** everything else — mid-level, junior, unleveled, or unknown titles.

For entries with **unknown title** (no `|` separators or blank title) → **KEEP** (evaluate with full JD).

### Mark SKIP entries in `data/pipeline.md`

Replace the `- [ ]` line with:
```
- [x] SKIP | {url} | {company} | {title} | title mismatch (seniority)
```

Do **not** move skipped entries to the Procesadas/Processed section — leave them inline with the SKIP marker so the user can review them.

---

## Step 4 — Evaluate Passing Entries

For each entry that passed the filter, run the full `auto-pipeline` (Blocks A–G evaluation → report `.md` → PDF if score ≥ 3.0 → TSV tracker addition).

**Parallelism rule:**
- **3+ passing entries** → launch parallel agents (`Agent` tool with `run_in_background: true`). Pass `_shared.md` + `auto-pipeline.md` content into each agent prompt.
- **< 3 passing entries** → process sequentially.

Each agent/sequential run:
1. Fetch JD (Playwright → WebFetch → WebSearch fallback)
2. Evaluate A–G (read `modes/_shared.md` + `modes/auto-pipeline.md`)
3. Save report to `reports/{###}-{company-slug}-{YYYY-MM-DD}.md`
4. Generate PDF if score ≥ 3.0 (run `modes/pdf.md` pipeline)
5. Write TSV to `batch/tracker-additions/{num}-{company-slug}.tsv`
6. Mark entry in `data/pipeline.md` as processed:
   ```
   - [x] #{num} | {url} | {company} | {title} | {score}/5 | PDF ✅/❌
   ```

After all parallel agents complete, proceed to Step 5.

---

## Step 5 — Merge Tracker

```bash
node merge-tracker.mjs
```

Consolidates all new TSV files from `batch/tracker-additions/` into `data/applications.md`.

---

## Step 6 — Summary Table

Output a summary of this scout run:

```
| # | Company | Role | Score | Rec | PDF | Report |
|---|---------|------|-------|-----|-----|--------|
| 1 | Acme    | ML Engineer | 4.3/5 | ✅ Apply | ✅ | [001](reports/001-...) |
| 2 | BetaCo  | AI PM | 3.1/5 | ⚠️ Borderline | ❌ | [002](reports/002-...) |
```

**Recommendation column:**
- Score ≥ 4.5 → `✅ Apply now`
- Score 4.0–4.4 → `✅ Apply`
- Score 3.5–3.9 → `⚠️ Borderline`
- Score < 3.5 → `❌ Skip`

Bottom-line summary:
```
X evaluated, Y worth applying (score ≥ 4.0), Z skipped as overqualified
```
