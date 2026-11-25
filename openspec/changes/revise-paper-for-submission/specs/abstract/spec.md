# Spec Delta

## Purpose

Defines what the paper's Abstract must contain so that it meets IEEE conference expectations while stating only planned or expected outcomes.

## ADDED Requirements

### Requirement: Abstract length
The Abstract SHALL be a single paragraph of 150 to 250 words, counted on the text between `\begin{abstract}` and `\end{abstract}` with LaTeX commands removed.

#### Scenario: Word count within range
- **GIVEN** the revised `main.tex`
- **WHEN** the words of the abstract body are counted
- **THEN** the count is at least 150 and at most 250

#### Scenario: Single paragraph
- **WHEN** the abstract body is inspected
- **THEN** it contains no blank line, list environment, or `\\` line break

### Requirement: Abstract content order
The Abstract SHALL present, in this order: (1) the problem and why it matters, (2) the proposed method, and (3) the expected contribution. Each part MUST be at least one full sentence.

#### Scenario: Problem stated first
- **WHEN** a reader reads the first two sentences of the abstract
- **THEN** they describe the hydration-monitoring problem or the limitations of current reminders, and they do not list hardware components

#### Scenario: Method follows problem
- **WHEN** the abstract is read in order
- **THEN** the description of the approach appears after the problem statement and before the contribution statement. It covers weight-based sensing, ML event recognition and prediction, and a context-aware agent with notification and feedback

#### Scenario: Contribution last
- **WHEN** the final sentences of the abstract are read
- **THEN** they state what the work is expected to contribute

### Requirement: Abstract makes no unsupported result claims
The Abstract MUST NOT state any measured number, accuracy, improvement, or user-study outcome. Outcomes SHALL be phrased as planned, expected, or proposed.

#### Scenario: No fabricated numbers
- **WHEN** the abstract is searched for percentages, F1, MAE, or comparative claims such as "outperforms" or "improves by"
- **THEN** none are present, unless they are explicitly framed as planned evaluation metrics

#### Scenario: Planned framing present
- **WHEN** the abstract mentions evaluation
- **THEN** it uses wording such as "we plan to evaluate" or "is expected to"

### Requirement: Abstract uses paper register
The Abstract MUST NOT describe the work as a course project or proposal. It MUST NOT contain placeholders, and it SHALL define any non-generic name (for example, LINE) the first time it is used.

#### Scenario: No course framing
- **WHEN** the abstract is searched for "final project", "proposal", "this project", or "[To be added"
- **THEN** none of these strings appear

#### Scenario: Product name defined
- **WHEN** "LINE" appears in the abstract
- **THEN** it is introduced as a messaging application, or it is replaced with a generic term such as "instant-messaging notifications"
