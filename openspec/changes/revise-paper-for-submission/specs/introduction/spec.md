# Spec Delta

## Purpose

Defines the structure and content of the paper's Introduction: motivation backed by real sources, explicit research questions, a contributions list, and a roadmap of the paper.

## ADDED Requirements

### Requirement: Motivation in prose with citations
The Introduction SHALL open with prose paragraphs that explain why hydration monitoring matters and why existing reminder approaches fall short. Every factual claim about health or prior systems MUST be supported by at least one `\cite{}` to an entry in `refs.bib`.

#### Scenario: No rhetorical-question or label openings
- **WHEN** the first paragraph of the Introduction is read
- **THEN** it does not start with a question, and the section contains no slide-style labels such as "Research motivation:"

#### Scenario: Claims are cited
- **GIVEN** a sentence that asserts a health fact or a property of existing systems
- **WHEN** it is checked
- **THEN** it ends with a `\cite{}` whose key exists in `refs.bib`

### Requirement: Three research questions stated explicitly
The Introduction SHALL state RQ1, RQ2, and RQ3 explicitly in the body text, each labelled and phrased as a question. Their meaning MUST stay consistent with the baseline RQs on sensing-based intake estimation, behavior prediction from history and context, and agent-driven reminder appropriateness.

#### Scenario: All three RQs present
- **WHEN** the Introduction is searched for "RQ1", "RQ2", and "RQ3"
- **THEN** each appears once as a labelled question, either inline or in an enumerated list, outside any table

#### Scenario: RQ meaning preserved
- **WHEN** the revised RQs are compared with Table I of the baseline
- **THEN** each revised RQ covers the same subject as the original, even if the wording is improved

### Requirement: Contributions list
The Introduction SHALL contain a list of contributions. The list MUST have between 3 and 5 items. Each item MUST be phrased as planned or expected, and MUST be traceable to the baseline "Expected Contributions" section.

#### Scenario: Contributions enumerated
- **WHEN** the Introduction is read
- **THEN** it contains an `itemize` or `enumerate` list introduced by a sentence such as "The expected contributions of this paper are:"

#### Scenario: No overclaiming
- **WHEN** each contribution item is read
- **THEN** none claims a demonstrated result, a measured improvement, or a released dataset that does not yet exist

### Requirement: Paper organization paragraph
The Introduction SHALL end with a paragraph that names the remaining sections in order, using `\ref{}` to section labels.

#### Scenario: Roadmap present
- **WHEN** the last paragraph of the Introduction is read
- **THEN** it mentions each later section, and every `\ref{}` in it resolves when the paper is compiled

### Requirement: No placeholders or course framing in Introduction
The Introduction MUST NOT contain placeholder text, "this project", or proposal or course framing.

#### Scenario: Placeholder removed
- **WHEN** the Introduction is searched for "[To be added", "TODO", "this project", or "final project"
- **THEN** none of these strings appear
