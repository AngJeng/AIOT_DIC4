# Proposal

## Why

`paper/before/main.tex` is a literal English conversion of a 12-slide project proposal. Its Abstract, Introduction and Related Work read like slide bullets, not like an IEEE conference paper, and would be rejected at desk review. This change brings those three sections up to submission quality. The rest of the paper stays as it is, and the paper still reports no measured results.

Problems observed in the current file (line numbers refer to `paper/before/main.tex`):

**Abstract (L39–41)**
- 134 words, below a typical IEEE conference abstract length (150–250).
- It opens with "This is an AIoT final project proposal…" (L40), which is course-report framing and not a research statement.
- It never states the problem or why it matters. It jumps straight to a list of components (cup, Raspberry Pi 4, AI agent, LINE).
- It describes the method as a list of parts ("weight sensing is used…, ML is used…") rather than an approach.
- Expected contributions are stated, but they are not connected to a problem.

**Introduction (L48–86)**
- It opens with a rhetorical question, "Why do we need smart hydration management?" (L50), followed by a semicolon list of claims with no supporting citations.
- L54, "Research motivation: …", is a slide-label fragment and not a sentence.
- L56 leaves the placeholder "[To be added: 2025–2026 public research…]" visible in the paper body.
- The three RQs appear only inside Table I (L61–77), mixed with planned techniques and metrics. They are never stated in prose.
- There is an "Objectives" list (L79–86) but no contributions list. The contributions appear only in a late "Expected Contributions" section.
- It has no paragraph describing how the paper is organized, and it says "this project" instead of "this paper".

**Related Work (L89–93)**
- It is one sentence (L91) that almost repeats Introduction L50 word for word.
- It has zero citations. `refs.bib` is empty, and `\bibliography` is commented out.
- Prior work is not grouped into categories, no approaches are compared, and no research gap is stated.
- The same visible placeholder appears again (L93).

## What Changes

- Rewrite the Abstract as a single paragraph of 150–250 words that covers problem → method → expected contribution, in that order. It uses planned/expected wording only.
- Restructure the Introduction:
  - motivation with real citations,
  - the limitations of existing approaches,
  - the proposed approach,
  - the three RQs stated explicitly,
  - a bulleted contributions list,
  - a paper-organization paragraph.
  
  Remove the slide fragments and the placeholders.
- Rewrite Related Work. It groups prior work into categories, compares them with the proposed system, and ends with an explicit research gap. Every citation is a real, verifiable publication.
- Populate `refs.bib` with verified entries only, and re-enable `\bibliographystyle{IEEEtran}` and `\bibliography{refs}`.
- Produce the revised paper as a separate "after" version. The "before" baseline is kept unchanged so the two can be compared.
- Make the edits through the Overleaf MCP and download the compiled "after" PDF.
- Out of scope: the title, the sections from System Architecture onward, the tables in later sections, the author block, and any new experiments or numbers.

## Capabilities

### New Capabilities
- `abstract`: requirements for the paper's Abstract, covering length, the problem/method/contribution order, and planned-only claims.
- `introduction`: requirements for the paper's Introduction, covering motivation, explicit RQs, the contributions list, paper organization, and no placeholders.
- `related-work`: requirements for the paper's Related Work, covering categorized comparison, the research gap, and real and verifiable citations only.

### Modified Capabilities
- (none — `openspec/specs/` is empty)

## Impact

- Files:
  - new `paper/after/main.tex` and `paper/after/refs.bib`;
  - new PDFs in `paper/pdf/`.
  
  `paper/before/` and `paper/before.zip` are not modified.
- External systems: Overleaf, accessed through the Overleaf MCP. This needs the MCP server to be connected, plus an Overleaf project.
- Citations: new entries must be checked by the user against DOI or publisher pages before the paper is considered final.
- No system code is affected.
