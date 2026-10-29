# Design rationale

This reference explains the outcomes protected by the technical-writing guidance and the tradeoffs involved in applying it. Read it when reviewing, extending, or adapting the skill. The operational instructions remain in `SKILL.md`; preserve their intended outcomes rather than applying a style convention mechanically.

## Audience awareness and clear purpose

**Intent:** Give readers information suited to their knowledge and the task, question, or decision that brought them to the document.

**Prevents:** Pages that bury the answer in background, omit necessary context, or try to address every audience equally.

**Adaptation boundary:** A document can support several related reader needs through clear navigation. One primary purpose does not require one audience or an exclusively procedural format. Infer scope from context and clarify consequential ambiguity.

## Accuracy and honest evidence

**Intent:** Make technical claims dependable and make the limits of their verification visible.

**Prevents:** Plausible but incorrect commands, mismatched versions, invented sources, and claims of successful execution based only on inspection.

**Adaptation boundary:** Match verification to the affected claims and the consequences of error. Source inspection can establish some facts; environment-dependent behavior may require execution. Disclose essential uncertainty rather than presenting it as settled behavior. A wording edit need not trigger unrelated system checks.

## Appropriate tone and reader empathy

**Intent:** Help readers proceed with confidence, including when they are unfamiliar with the system or recovering from a failure.

**Prevents:** Condescension, blame, promotional language, and reassurance that dismisses real difficulty.

**Adaptation boundary:** Tone may be welcoming in onboarding, neutral in reference material, and terse in an incident runbook. Respect and precision remain constant. Encouragement is useful when it helps the reader; words such as "just" or "obviously" often conceal assumptions instead.

## Plain language and consistent terminology

**Intent:** Reduce interpretation effort while preserving technical meaning. Keep the vocabulary of documentation, product interfaces, and reader conversations aligned.

**Prevents:** Jargon without explanation, vague instructions, and synonyms that make one concept look like several different concepts.

**Adaptation boundary:** Plain language does not mean removing necessary technical terms. Define them for the intended audience and preserve exact identifiers and interface labels. Sentence length should follow the relationship between ideas, not a fixed limit on facts or clauses.

## Logical structure, coherence, and findability

**Intent:** Help readers locate an answer and understand how the information fits together.

**Prevents:** Clear individual sentences arranged in an order that forces readers to reconstruct the argument or procedure.

**Adaptation boundary:** Procedures favor ordered actions; references favor lookup; conceptual explanations favor relationships and reasoning. Follow the document's purpose and established conventions rather than treating the structure table as a mandatory template. Headings should distinguish useful entry points.

## Actionability and sufficient completeness

**Intent:** Enable readers to act correctly, recognize success, and recover from likely failures without guessing essential steps.

**Prevents:** Missing prerequisites, unexplained choices, buried warnings, and instructions that stop before readers can verify the result.

**Adaptation boundary:** Completeness is relative to scope. Include conditions that change correct use; link specialist detail and unlikely cases when appropriate. Conceptual documentation may support a decision or understanding rather than a runnable procedure. Repeat critical conditions where independently used sections need them.

## Explanatory value and useful examples

**Intent:** Give readers enough understanding to apply guidance beyond the exact example and to make informed choices.

**Prevents:** Recipes that work only when copied verbatim, abstract explanations without practical meaning, and fragments mistaken for complete programs.

**Adaptation boundary:** Explain why when it changes reasoning, troubleshooting, or decisions. A routine command may need no background; an architectural tradeoff may need substantial rationale. Choose representative, verified examples and identify simplifications that affect their use.

## Concision without loss of meaning

**Intent:** Keep attention on information that changes understanding or action.

**Prevents:** Repetitive summaries, filler, irrelevant background, and documents whose length disguises an incomplete answer.

**Adaptation boundary:** Shorter is useful only while the document remains complete. Preserve prerequisites, caveats, and explanatory context. Deliberate repetition can help independently read sections. Avoid word-count targets that reward deleting necessary meaning.

## Accessible and inclusive presentation

**Intent:** Make information usable across different reading methods, language backgrounds, and access needs.

**Prevents:** Ambiguous links, essential information available only in an image or color cue, and unnecessary cultural assumptions.

**Adaptation boundary:** Keep technical precision while explaining unfamiliar terms and using descriptive navigation. Meaningful visuals need a text alternative or adjacent explanation that communicates their essential content; duplicating every decorative detail adds no value.

## Useful visual communication

**Intent:** Make relationships, comparisons, and processes easier to understand when prose alone would demand more effort.

**Prevents:** Decorative diagrams, unnecessary tables, and visuals that contradict the surrounding explanation or become stale independently.

**Adaptation boundary:** Choose the format by the reader's question. Tables suit aligned comparisons; diagrams suit relationships and flow. A short paragraph can be clearer than either. Treat labels and visual content as technical claims requiring verification and maintenance.

## Maintainability

**Intent:** Keep documentation trustworthy as the system and its audience change.

**Prevents:** Conflicting copies of shared contracts, unclear version applicability, and instructions that persist after their assumptions become obsolete.

**Adaptation boundary:** Keep authoritative details with their owner and link to them, while retaining necessary context at the point of use. Preserve known ownership and review triggers where ongoing upkeep matters. Do not invent owners, impose maintenance ceremony on every page, or treat a recent review date as proof of accuracy.
