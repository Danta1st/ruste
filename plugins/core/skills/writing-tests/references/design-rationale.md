# Design rationale

This reference is for maintainers and for agents deciding whether local constraints justify adapting the writing-tests guidance. The operational rules remain in `SKILL.md`; this document explains the outcomes those rules protect. Preserve those outcomes rather than copying a convention mechanically or weakening it for convenience.

## Treat tests as executable functional requirements

**Intent:** Make the automated suite a durable, inspectable account of agreed system behavior. Product Owners and domain experts should be able to compare the named scenarios with acceptance criteria and identify omissions.

**Prevents:** A suite that proves implementation details while leaving uncertainty about which user or business requirements it covers.

**Adaptation boundary:** Infrastructure-level tests may protect technical contracts, but their names should still state the stable observable contract rather than the code path used to implement it. If a test cannot name a meaningful behavior or invariant, reconsider whether it provides durable value.

## Use domain language and PO-readable names

**Intent:** Keep requirements, conversations, and tests in one vocabulary. Shared language reduces translation errors and makes changes in meaning visible.

**Prevents:** Tests that are understandable only to the author because they use class names, method names, storage terminology, or team-local abbreviations instead of domain concepts.

**Adaptation boundary:** Follow the target language's identifier constraints, but do not trade away the domain meaning. A local naming style may change punctuation or casing without changing the behavioral sentence.

## State action, relevant condition, and observable outcome

**Intent:** Make each title precise enough to distinguish one requirement from nearby cases and useful enough to diagnose a failing behavior from test output.

**Prevents:** Vague titles such as `works`, assertion-shaped titles such as `returns_true`, and names that describe setup without saying what the system must do.

**Adaptation boundary:** This is a semantic shape, not a rigid title template. Omit a component when it is already unambiguous, and prefer a natural behavioral sentence over mechanically inserting `Given`, `When`, or `Then` into the name.

## Keep technical mechanisms out of titles

**Intent:** Separate the requirement from the current evidence used to prove it. The title should survive changes to transports, persistence, internal APIs, and error representation.

**Prevents:** Routine refactoring from making test reports obsolete, and implementation choices from being mistaken for product requirements.

**Adaptation boundary:** A technical detail may be asserted in the body when it is part of a public contract. Name the user- or consumer-visible consequence unless the technical contract itself is the stable behavior under test.

## Discover Features, Rules, and Examples

**Intent:** Make behavior-driven development a progression from valuable capability, through explicit business rules, to concrete examples that can guide implementation and become living documentation.

**Prevents:** Given/When/Then syntax being added after implementation without first discovering what rule the example proves, and production structure dictating the scenario inventory.

**Adaptation boundary:** Use a separate behavior map only when several examples, ambiguity, or stakeholder discussion justifies it. One clear requirement can be captured completely by its named executable scenario. The terminology does not require Cucumber or `.feature` files.

## Express behavior with Gherkin semantics

**Intent:** Expose the scenario's business-relevant context, single external stimulus, and observable result independently of setup helpers and assertion syntax. Concise, searchable labels make the suite a scenario index for humans and AI agents.

**Prevents:** Tests whose purpose can be reconstructed only by tracing fixtures, calls, and assertions; comments that merely translate code into prose; and repeated setup narration that hides the behavioral signal.

**Adaptation boundary:** Behavior-facing scenarios use concise `Given`, `When`, and `Then` clauses. `And` and `But` may continue the preceding role but must not hide another action or behavior. Use `Background` only for short shared domain context and `Scenario Outline` only when every row illustrates the same Rule. Narrow technical tests may omit scenario comments when they add no semantic value, but must still name the stable rule or contract.

## Keep agreed examples visible until implemented

**Intent:** Separate discovering the required scenarios from implementing them. Agreeing on named cases first anchors scope, reveals missing cases, and makes progress visible.

**Prevents:** Production structure from dictating the test inventory, late discovery of misunderstood acceptance criteria, and coding only the happy path because it was easiest to reach.

**Adaptation boundary:** A separate ledger is useful for multiple or ambiguous examples, not mandatory ceremony for one clear requirement. Use pending executable scaffolds only when several agreed cases need visible implementation status and the project has a safe convention for them. Otherwise keep incomplete cases in a non-executable plan. Never retrofit unrelated tests or create a false green build.

## Implement one scenario at a time

**Intent:** Keep each implementation step tied to an agreed outcome and obtain feedback before several assumptions become entangled.

**Prevents:** Large speculative implementations, broad test rewrites, and failures whose responsible behavior is difficult to isolate.

**Adaptation boundary:** Closely related examples may be implemented together when the framework represents them as one parameterized requirement. The grouping should improve clarity, not hide distinct acceptance criteria.

## Begin coverage with requirements, not lines

**Intent:** Define adequate testing by the behavior and risks the system must address. Structural coverage is useful only after the functional inventory is understood.

**Prevents:** High coverage numbers creating false confidence while important outcomes, error cases, or boundary conditions remain untested.

**Adaptation boundary:** Use line and branch coverage to find suspicious gaps or dead paths. Do not use a percentage as evidence that the requirements ledger is complete.

## Test from representative system edges

**Intent:** Start from an externally observable requirement, then prove that a representative user, system, or environmental stimulus travels through production-like composition and produces the promised result. Edge tests cover behavior plus the wiring needed to deliver it.

**Prevents:** A collection of locally correct components that fail when composed, and tests shaped around classes, methods, files, interfaces, or collaborator sequences rather than product behavior.

**Adaptation boundary:** Edge-first does not mean choosing the largest possible end-to-end environment or real infrastructure. Use the smallest boundary that faithfully exercises the requirement. Move inward with a clear reason when an isolated domain rule, algorithm, parser, protocol or serialization contract is clearer and faster in isolation, or when an edge test would be disproportionately costly or diagnostically weak.

## Substitute only genuine boundaries

**Intent:** Keep internal collaboration free to change while controlling dependencies outside the application's deterministic control, such as third-party services, transports, payment providers, or time.

**Prevents:** Mock call choreography from freezing the internal design and producing tests that pass even when the delivered outcome is wrong.

**Adaptation boundary:** A fake, simulator, or stub is justified by the boundary it replaces, not by the convenience of injecting it. Use it to supply realistic external stimuli or capture meaningful effects. Verify an interaction only when that interaction itself is the externally observable contract.

When the repository provides reliable test-hosted infrastructure for components inside the selected boundary, using that established composition often gives stronger evidence than mocking internal interfaces. This preference does not require live third-party dependencies or override the smallest-boundary rule.

## Test structure only when structure is the contract

**Intent:** Let implementations be refactored freely while preserving explicit names, schemas, shapes, and metadata that real consumers require.

**Prevents:** Reflection assertions, file-layout checks, snapshots, or internal wiring tests from turning incidental design choices into permanent requirements.

**Adaptation boundary:** Structure is legitimate test subject when users, tooling, dependency injection, serialization, plugin discovery, or another external party consumes it. Name that consumer contract and observe it through the relevant boundary when practical. Protect only the required shape; if externally observable behavior can remain correct while the test fails, the assertion is probably too coupled.

## Keep framework mechanics in conditional references

**Intent:** Preserve one cross-language testing philosophy while allowing correct, current syntax for a consuming project's tools and versions.

**Prevents:** Framework details overwhelming ordinary guidance, version-sensitive syntax becoming universal policy, and one repository's architecture being imposed on another.

**Adaptation boundary:** Installed dependencies, executable code, and established project conventions decide the mechanics. Framework references may explain how to express the shared outcomes, but they must not redefine those outcomes.
