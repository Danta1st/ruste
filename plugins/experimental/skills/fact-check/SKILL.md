---
name: fact-check
description: Assess externally verifiable claims in text, links, or other supplied material. Use when the user asks to fact-check, verify, validate, or investigate a claim.
---

# Fact Check

Assess evidence for discrete factual claims without overstating certainty.

## Input

If the user has not provided text, claims, links, or other material, ask for what they want checked. Do not say content is verified without evidence.

## Workflow

1. Identify discrete claims that can be externally verified. Separate facts from opinions, predictions, and ambiguous statements. Mark the latter as out of scope or ask for clarification when a neutral restatement cannot resolve the ambiguity.
2. When live research is available, prefer current primary or official sources. For consequential claims, corroborate with independent sources where practical. Record the source publication date and relevant event date so historical and current claims are not conflated.
3. When live research is unavailable, say so clearly. Assess only the material and sources the user supplied; do not invent verification or sources.
4. Give each factual claim one evidence-based verdict:
   - **Supported** — direct, reliable evidence supports the claim.
   - **Contradicted** — reliable evidence conflicts with the claim.
   - **Misleading** — the claim omits or distorts material context.
   - **Unverifiable** — available evidence is insufficient to assess it.
   - **Out of scope** — opinion, prediction, or unresolved ambiguity rather than a checkable factual claim.
5. Place an explicit, direct source link next to every finding. Missing evidence is not disproof; use **Unverifiable** rather than **Contradicted** when evidence is absent or insufficient.

For medical, legal, financial, or other high-stakes topics, describe only what the available evidence supports and its limitations. Do not diagnose, advise, or make conclusions beyond that evidence.

## Response Format

### Overall assessment

One or two sentences describing the scope and balance of the evidence.

### Findings

| Claim | Verdict | Evidence |
|---|---|---|
| [Discrete claim] | Supported, contradicted, misleading, unverifiable, or out of scope | [Source title](https://example.com) — publication date; event date when relevant |

### Limitations

- State unavailable live research, limited source access, unresolved ambiguity, or other evidence gaps.

### Corrections or suggested wording

- Offer a precise correction or a more qualified wording when the evidence supports one.
