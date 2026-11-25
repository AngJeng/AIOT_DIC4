# Tasks

## 1. Preparation

- [x] 1.1 The "after" version is edited directly in the Overleaf project "Smart Hydration Monitoring Agent: …" (id `6b06594961300f919abe37fb`), not as a local copy first. `paper/after/` is created only at the end (task 6.1), from `read_file` output. `paper/before/` is never modified
- [x] 1.2 Overleaf access uses `overleaf-web-mcp` (free plan, no git). Edits use `list_projects`, `get_project_tree`, `get_sections`, `get_section_content`, `read_file`, and `write_section` (`write_file` only for `refs.bib`). Destructive tools (`delete_entities`, `manage_entity` delete, `sync_directory`, `manage_project`) are not used. Manual steps by the user: none so far
- [ ] 1.3 The baseline Overleaf project "AIOT_DIC4_before" (DIC4-before) is left untouched. Compile it and download `paper/pdf/before.pdf` (by MCP or manually); verify that the PDF opens and shows the baseline Abstract
- [x] 1.4 Read `main.tex` from the after project through the MCP and verify that it matches the local `paper/before/main.tex` (verified on 2026-11-25: the SHA-256 values match, `be5c1b3b…49db0d`)

## 2. References (prerequisite for Related Work and Introduction)

- [x] 2.1 Search for candidate papers for each category: sensor-based intake monitoring, hydration reminders / just-in-time interventions, and LLM/agent health assistants. Verify that at least 2 candidates per category have a confirmed DOI or official page
- [x] 2.2 Write `openspec/changes/revise-paper-for-submission/refs-verification.md` listing key, authors, title, venue, year, DOI/URL, category, and a "verified by user" checkbox for each entry; verify that every row has a resolvable DOI/URL
- [ ] 2.3 Write the verified entries to `refs.bib` in the Overleaf project, and un-comment `\bibliographystyle{IEEEtran}` and `\bibliography{refs}`; verify that Overleaf compiles without BibTeX errors

## 3. Related Work (specs/related-work)

- [ ] 3.1 Through the MCP, rewrite Related Work with at least 3 category subsections, each citing ≥2 verified works; verify with the "Categories present" scenario
- [ ] 3.2 Add the comparison on sensing, automatic intake estimation, context use, and adaptive reminders; verify that the proposed system is described as design properties, with no measured claims
- [ ] 3.3 Add the closing research-gap paragraph that links to RQ1–RQ3; verify the "Gap paragraph" scenario and that no "[To be added" or "TODO" remains
- [ ] 3.4 Compile on Overleaf; verify the log has no undefined citations and the References section lists every cited work

## 4. Introduction (specs/introduction)

- [ ] 4.1 Add `\label{sec:…}` to each later section (no other change to those sections) and compile; verify the labels exist
- [ ] 4.2 Through the MCP, rewrite the motivation and limitations paragraphs as cited prose; verify there is no opening question, no "Research motivation:" label, and that each factual claim has a `\cite{}`
- [ ] 4.3 State RQ1–RQ3 as an enumerated list in the body text and keep Table I referenced; verify that each RQ appears once and keeps its original subject
- [ ] 4.4 Replace the "Objectives" subsection with a 3–5 item expected-contributions list; verify that no item claims a measured result
- [ ] 4.5 Add the paper-organization paragraph with `\ref{}`s, then compile; verify there are no "??" in the PDF and no "this project", "final project", or "[To be added" in the Introduction

## 5. Abstract (specs/abstract)

- [ ] 5.1 Through the MCP, rewrite the Abstract in the order problem → gap → method → planned evaluation → expected contribution; verify the order with the "Problem stated first", "Method follows problem", and "Contribution last" scenarios
- [ ] 5.2 Count the words of the abstract body; verify that the count is between 150 and 250 and the abstract is a single paragraph
- [ ] 5.3 Scan the abstract for numbers, "outperform", "improve by", "proposal", and "this project"; verify that none are present and that LINE is either defined or generalized

## 6. Integration check and delivery

- [ ] 6.1 Pull the final `main.tex` and `refs.bib` from Overleaf into `paper/after/`; verify that a diff against `paper/before/main.tex` changes only the Abstract, Introduction, Related Work, `\label` lines, and bibliography lines
- [ ] 6.2 Run `openspec validate revise-paper-for-submission --strict` and re-check every scenario in the three specs against `paper/after/main.tex`; record pass/fail in this file
- [ ] 6.3 Give the user the verification list from `refs-verification.md` for manual citation checks
- [ ] 6.4 Download the revised PDF from Overleaf to `paper/pdf/after.pdf`; verify that it opens and that its Abstract, Introduction, and Related Work match `paper/after/main.tex`

## Workflow follow-up

- After the user has verified all citations, archive the change so that the abstract, introduction, and related-work specs move into `openspec/specs/`.
