# Spec Delta

## Purpose

Defines how the paper's Related Work section groups and compares prior approaches, identifies the research gap, and guarantees that every citation refers to a real, verifiable publication.

## ADDED Requirements

### Requirement: Categorized discussion of prior work
Related Work SHALL group prior work into at least three categories, each in its own `\subsection` or clearly separated paragraph. The categories SHALL include sensor-based intake monitoring, hydration or health reminder and intervention approaches, and LLM- or agent-based personal health assistants.

#### Scenario: Categories present
- **WHEN** Related Work is read
- **THEN** it has at least three titled categories, and each category cites at least two works

### Requirement: Comparison with the proposed system
Related Work SHALL compare the existing approaches with the proposed system along explicit dimensions: sensing modality, automatic intake estimation, use of context (exercise/diet), and adaptive or personalized reminders. The comparison MAY be written as prose or as a table.

#### Scenario: Comparison dimensions stated
- **WHEN** the comparison is read
- **THEN** each category is characterized on the named dimensions, and the proposed system's position on the same dimensions is stated as a design property, not as a measured result

### Requirement: Explicit research gap
Related Work SHALL end with a paragraph that states the research gap this paper addresses and links that gap to RQ1–RQ3.

#### Scenario: Gap paragraph
- **WHEN** the final paragraph of Related Work is read
- **THEN** it contains an explicit gap statement (for example, "To the best of our knowledge, no prior work…" or "However, existing approaches…") and references at least one RQ

### Requirement: Only real, verifiable citations
Every citation in Related Work MUST correspond to a publication that exists. Each `refs.bib` entry MUST include a DOI, or an official URL when no DOI exists. Entries MUST NOT be invented, and author, title, venue, and year MUST match the source.

#### Scenario: Each entry verifiable
- **GIVEN** any entry in `refs.bib`
- **WHEN** its DOI or URL is opened
- **THEN** it resolves to a publication whose authors, title, venue, and year match the entry

#### Scenario: Unverifiable source excluded
- **GIVEN** a candidate source whose existence or metadata cannot be confirmed
- **WHEN** references are compiled
- **THEN** that source is not added to `refs.bib` and is not cited

#### Scenario: Verification list for the user
- **WHEN** the revision is delivered
- **THEN** the user receives a list of every `refs.bib` key with its title and DOI/URL for manual checking

### Requirement: Citations compile cleanly
All `\cite{}` keys used in the paper MUST exist in `refs.bib`. The bibliography SHALL be rendered with `\bibliographystyle{IEEEtran}`.

#### Scenario: No undefined citations
- **WHEN** the paper is compiled on Overleaf
- **THEN** the log contains no "Citation … undefined" warning, and the References section lists every cited work

### Requirement: No duplication or placeholders in Related Work
Related Work MUST NOT repeat the Introduction's motivation sentences word for word, and MUST NOT contain placeholder text.

#### Scenario: No placeholder
- **WHEN** Related Work is searched for "[To be added" or "TODO"
- **THEN** neither string appears
