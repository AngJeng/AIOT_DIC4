# Design

## Context

- The baseline is `paper/before/main.tex`, an IEEEtran conference paper converted faithfully from the proposal slides. `paper/before/refs.bib` has no entries, and `\bibliography` is commented out. The proposal (see "Why") lists the specific problems.
- No LaTeX toolchain is installed locally, so compilation must happen on Overleaf.
- The edits are to be made by the agent through the Overleaf MCP. At planning time no Overleaf MCP tools are connected in this session; they must be connected before the apply phase.
- The paper has no measured data. Every outcome must remain planned or expected (see `specs/abstract`, `specs/introduction`).

## Goals / Non-Goals

**Goals:**
- Produce `paper/after/` with the Abstract, Introduction, and Related Work revised to meet the three specs.
- Keep `paper/before/` byte-identical, so the before and after versions can be compared.
- Obtain compiled PDFs of both versions in `paper/pdf/`.

**Non-Goals:**
- Changing the content of System Architecture through Conclusion. The only exception is adding `\label{sec:…}` lines that the Introduction's roadmap needs.
- Adding experiments, numbers, figures, or new claims about the system.
- Polishing the title or the author block.

## Decisions

### D1. Separate "after" copy, baseline untouched
Copy `paper/before/` to `paper/after/` and edit only the copy, in a separate Overleaf project.
- *Alternative:* edit `before` in place and rely on git history. Rejected because the assignment needs both versions side by side as files and PDFs.

### D2. Overleaf as the editing and compile surface; local files as the record
Workflow:
1. Create an Overleaf project named "DIC4-after" from the `paper/after` files. If the MCP cannot create projects, the user creates it by uploading `paper/after.zip`, and the agent then opens it through the MCP.
2. Through the MCP, read `main.tex` and confirm it matches the local copy.
3. Edit one section per step, in this order: `refs.bib`, then Related Work, then Introduction, then Abstract. Related Work comes first because the Introduction cites the same works and the Abstract summarizes the Introduction.
4. After each section edit, trigger a compile through the MCP and read the log. Fix errors before moving to the next section.
5. Pull the final `main.tex` and `refs.bib` back into `paper/after/`, and download the PDF to `paper/pdf/after.pdf`.

Also compile the unmodified baseline in its own Overleaf project and download `paper/pdf/before.pdf`.
- *Alternative:* edit locally and upload once. Rejected because the assignment requires the agent to make the edits through the Overleaf MCP, and per-section compiles catch errors early.

### D3. Citation sourcing and verification
- Search for candidate papers for each Related Work category.
- Confirm each candidate's metadata against its DOI landing page or the publisher or indexing page, and add it to `refs.bib` only after that check.
- Prefer peer-reviewed venues (IEEE, ACM, Elsevier, Springer, MDPI) and DOI-bearing entries.
- Record every entry in `openspec/changes/revise-paper-for-submission/refs-verification.md` with: key, authors, title, venue, year, DOI/URL, the category it supports, and a "verified by user" checkbox.
- Search queries contain only topic keywords, never personal data.
- Target: 8–15 references, with at least two per Related Work category.
- *Alternative:* ask the user to supply all references. Rejected as the default because it is slower, but the user's own references are welcome and will go through the same verification.

### D4. Section structures

**Abstract** (one paragraph, 150–250 words):
1. Problem: dehydration and why it matters, plus the limitations of manual logging and fixed-time reminders (1–2 sentences).
2. Gap: existing systems rarely combine automatic intake sensing with context-aware, adaptive reminders (1 sentence).
3. Method: a low-cost load-cell cup with a Raspberry Pi; ML for drinking-event recognition and next-hour drinking prediction; an LLM-based agent that uses tool calls to decide reminder timing and sends instant-messaging notifications that collect feedback (2–3 sentences).
4. Planned evaluation: threshold vs. ML event recognition, and fixed vs. ML vs. ML+agent reminder strategies, with the metrics named (1–2 sentences).
5. Expected contributions (1–2 sentences).

**Introduction:**
- P1: why hydration matters (cited).
- P2: limitations of current practice, namely manual logging, fixed reminders, and no context (cited).
- P3: how AIoT, ML, and LLM agents enable a different approach, and an overview of the proposed system.
- P4: the RQs as an `enumerate` of RQ1–RQ3. Keep Table I as the mapping from RQ to planned experiment and metric, and refer to it.
- P5: "The expected contributions of this paper are:" followed by an `itemize` of 3–5 items drawn from the baseline "Expected Contributions". This replaces the "Objectives" subsection.
- P6: a roadmap paragraph with `\ref{}` to the later sections.

**Related Work:**
- A. Sensor-based fluid intake monitoring: smart bottles and cups (weight, capacitive), wearables, vision.
- B. Hydration reminders and just-in-time health interventions: fixed reminders, apps, adaptive interventions.
- C. LLM- and agent-based personal health assistants.
- D. A short comparison paragraph or a small table on four dimensions: sensing, automatic intake estimation, context use, and adaptive reminders.
- E. A gap paragraph that links to RQ1–RQ3.

### D5. Overlap with the later "Expected Contributions" section
That section is out of scope, so it remains. The Introduction's contribution list is written as concise claims, so the duplication is acceptable for a conference paper. Removing the later section is deferred to a separate change, if wanted.

## Risks / Trade-offs

- [Overleaf MCP not connected or lacking create/compile/download operations] → Check the available tools before editing. Have the user create the project or download the PDF manually where the MCP cannot, and record which steps were manual.
- [Hallucinated or wrong citation metadata] → Follow D3. Nothing enters `refs.bib` without checking its DOI or source page. The user also checks every entry using `refs-verification.md`.
- [Too few verifiable sources in a category] → Merge categories or reduce the minimum, and tell the user. Never pad with unverifiable entries.
- [Overclaiming while polishing] → Before delivery, scan the three sections for numbers and comparative verbs (spec scenarios).
- [Abstract word-count drift during edits] → Re-count after the final edit.
- [Edits leaking into out-of-scope sections] → Diff `paper/after/main.tex` against `paper/before/main.tex`. Only the three sections, `\label` additions, and the bibliography lines may differ.

## Open Questions

- The exact Overleaf MCP tool names and whether they support project creation and PDF download. This affects only which steps are manual, not the plan.
