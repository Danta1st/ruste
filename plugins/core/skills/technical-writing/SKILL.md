---
name: technical-writing
description: Writes, reviews, and improves technical documentation such as READMEs, API references, setup guides, runbooks, and code comments. Use when the user asks to create or change documentation, not for general questions or code analysis.
---

# Technical Writing

Help the intended reader understand a system, complete a task, or make a decision correctly. Aim for the shortest complete explanation that serves that purpose.

When reviewing, extending, or adapting these conventions, read [Design rationale](references/design-rationale.md) to understand the intended outcomes and legitimate adaptation boundaries. Do not load it for ordinary documentation tasks.

## Establish audience, scope, and evidence

Identify the reader, their goal, and necessary prior knowledge. Infer these from context; state consequential assumptions when unclear. Follow the repository's documentation conventions and keep edits within the requested scope.

Check technical claims, commands, and flags against source, configuration, CLI help, schemas, tests, or authorized execution. Match API signatures and version claims to installed dependencies. For external APIs, use current official documentation through Context7 or Microsoft Learn when relevant and available, with official web documentation as a fallback. Executable code and tests take precedence over conflicting prose.

Distinguish verified facts, inferences, and unknowns. Never imply execution when only inspection occurred. Disclose essential evidence gaps and request missing evidence when needed; omit nonessential speculation. Cite evidence near material or disputed claims using the document's conventions. Never include secret values.

## Write for the reader

- **Purpose and relevance:** Make the document's purpose, scope, and intended outcome apparent early. Include context that helps the reader act, understand, or decide.
- **Tone and empathy:** Be calm, direct, and respectful; adapt formality to the audience and situation. Avoid hype, blame, condescension, and forced friendliness. Anticipate confusion and recovery needs without assuming incompetence.
- **Language and precision:** Prefer familiar words and concrete verbs. Use technical terms when they improve precision, define unfamiliar terms, and match product labels. Keep terminology consistent; avoid idioms and unnecessary cultural assumptions. Use imperative verbs for instructions.
- **Structure and coherence:** Organize around reader questions and lookup needs. Use descriptive headings and connected paragraphs; put main behavior before exceptions. Keep required conditions and warnings beside the actions they govern, repeating them when sections must work independently.
- **Actionability and completeness:** Include necessary prerequisites, ordered actions, constraints, expected results, and success checks. Address likely failures and recovery where relevant. Explain consequential choices rather than leaving readers to guess.
- **Explanation and examples:** Explain why when it helps readers reason, troubleshoot, or choose correctly. Use realistic, verified examples beside the behavior they illustrate; identify fragments and show relevant inputs and outputs.
- **Concision:** Remove filler, repetition, and detail that changes neither understanding nor action. Preserve necessary explanations and caveats; link optional depth. Let sentence and paragraph length serve clarity.
- **Accessibility and presentation:** Use descriptive links and text alternatives for meaningful visuals. Use diagrams and tables when they reduce cognitive effort; label them clearly and keep them consistent with the prose. Format identifiers as inline code and executable examples as code blocks. Use bullets for parallel items and numbered lists for ordered steps.
- **Maintainability:** Keep shared contracts in their owning document and link to them. Preserve relevant ownership and version context. For documentation that needs ongoing upkeep, identify an owner or review trigger when known; do not invent them.

## Choose a structure that fits the purpose

Use these as defaults; follow established templates and omit inapplicable sections.

| Document | Useful structure |
|---|---|
| README / getting started | purpose → prerequisites and setup → smallest working example → next steps |
| Concept / domain overview | purpose → concepts and relationships → rules, rationale, and boundaries → further detail |
| How-to / setup guide | prerequisites and required configuration → ordered actions → success check |
| API / config / CLI reference | signature or schema → required and optional inputs, defaults, and constraints → outputs and errors → example |
| Runbook / troubleshooting | symptom → diagnostic checks → conditional fixes → confirmation; rollback and escalation where applicable |

## Review before delivery

Check new or affected commands, paths, examples, outputs, and claims at a level appropriate to the edit; distinguish inspection from execution and disclose remaining uncertainty. Then assess whether a representative reader can achieve the intended outcome using the document and its links, with enough explanation to make necessary decisions. Check terminology, tone, navigation, and accessibility. Remove anything that adds no useful meaning.
