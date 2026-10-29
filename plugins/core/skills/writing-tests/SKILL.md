---
name: writing-tests
description: Design and write tests as PO-readable executable requirements using domain language, concrete behavioral examples, and outside-in testing. Use when adding, changing, or reviewing automated tests or test-first feature work.
---

# Writing Tests

Treat tests as the executable record of agreed behavior. Their names and scenarios should let a non-technical Product Owner understand what the system does, confirm covered requirements, and notice missing ones.

When reviewing, extending, or adapting these conventions, read [Design rationale](references/design-rationale.md) to understand the intended outcomes and legitimate adaptation boundaries. Do not load it for ordinary test-writing tasks.

## Discover and formulate behavior

Read the acceptance criteria, domain vocabulary, and existing test conventions before choosing test mechanics. If expected behavior is ambiguous, surface the ambiguity instead of encoding an assumption.

For a non-trivial or ambiguous change, organize the discussion as:

- **Feature:** the user capability or valuable outcome;
- **Rule:** one business policy or invariant within that capability; and
- **Example:** a concrete case that illustrates the rule and can become an executable scenario.

For example: Feature `Order cancellation` → Rule `Orders may be cancelled until fulfilment begins` → Examples for requests made before and after fulfilment begins.

Use a lightweight behavior map or requirements ledger only when several examples, unclear rules, or stakeholder discussion make it useful. For one clear requirement, the named test can be the complete record. Do not retrofit unrelated tests merely because this skill is active.

Map each agreed Example to a PO-readable test name before writing production code. Create pending scaffolds only when they help keep multiple agreed but unimplemented examples visible. Never leave a runnable empty body that passes; use the project's explicit pending convention or keep the example in a non-executable plan. Implement one Example and the minimum production behavior at a time.

Start coverage discussions from Features, Rules, concrete Examples, and meaningful edge cases. Use line or branch coverage later as a diagnostic, not as the definition of adequate behavior.

## Name executable requirements

Use the domain's ubiquitous language: the words users, Product Owners, and domain experts use. Favor a title that states an action, any relevant condition, and an observable outcome, such as `Customers_can_cancel_orders_before_fulfilment`.

A good title:

- describes a capability, rule, state change, or consequence;
- remains true after internal refactoring;
- can be copied into a requirements document; and
- follows the target language's identifier conventions.

Keep technical mechanisms out of titles: production method or class names, routes, HTTP status codes, exception types, database operations, and mock interactions belong in the test body when relevant.

For example:

- `Users_cannot_unsubscribe_from_mandatory_interest_areas`
- `Making_an_interest_area_mandatory_auto_subscribes_all_users`
- `Deleting_an_interest_area_removes_all_related_subscriptions`

Avoid `PatchSubscriptions_UnsubscribeMandatory_Returns400`, `EnableMandatory_AutoSubscribesNonSubscribedUsers`, and `DeleteInterestArea_CascadesSubscriptions` because they describe implementation rather than the delivered behavior.

For C# projects using NUnit, read [C# and NUnit](references/csharp-nunit.md) before choosing names, attributes, async signatures, categories, or pending-test syntax.

## Write readable scenarios

Every behavior-facing scenario must make its semantics immediately visible through concise Gherkin-inspired comments using the target language's normal comment syntax and these recognizable, searchable labels:

```text
// Given: business-relevant starting context or externally supplied condition
// When: the single user or external-system stimulus entering through the chosen edge
// Then: the observable business outcome or external contract
```

Prefer one concise line per clause. Use `And` or `But` only for an independently meaningful condition or outcome that continues the preceding semantic role; do not use either to hide a second action or behavior. Use domain language and concrete outcomes rather than fixture mechanics, transport plumbing, mocks, API calls, or individual assertions. Treat the comments as a compact scenario index that an AI can search and a human can scan, not as a line-by-line translation of the test.

Keep all three labels so the scenario exposes missing context, stimulus, or outcome. The test name and comments should complement one another rather than repeat mechanically: the name identifies the executable requirement in test reports, while the comments make context → stimulus → outcome visible in source.

Use `Scenario Outline` and `Examples` as the model for a parameterized test only when every row is another concrete Example of the same Rule. Use `Background` only for short, business-relevant context shared by every Example in a group; never let shared setup hide context needed to understand an individual scenario. These broader organizational terms guide discovery and do not require Cucumber, `.feature` files, or matching literal comments in test code.

A narrow test for an algorithm, parser, protocol, serialization contract, or similar technical invariant may omit scenario comments when behavioral narration adds no useful meaning. Its name must still state the stable rule or contract it protects.

## Test from the edges inward

Start from the requirement or scenario, not the production class structure. Choose the outermost practical entry point that proves it, using production-like composition and input from a user, collaborating system, or genuine environment. Then verify what that actor can observe. Use the smallest boundary that still proves the requested behavior and remains resistant to internal refactoring; outside-in does not require a slow, full-stack end-to-end environment or real external infrastructure.

For backend behavior, representative entry points include REST/HTTP or RPC requests, incoming bus messages or events, commands, scheduled-job triggers, CLI input, and other externally initiated adapters. Verify externally meaningful responses, emitted messages, persisted or queryable state, or other contractual effects.

For frontend behavior, drive the application through user input and events, or through realistic simulated server responses, pushed notifications or events, routing, time, and other genuine environmental stimuli. Verify what the user can perceive or what the external contract exposes, not component state or implementation details.

Use a narrow unit test when it gives better evidence for an isolated domain rule, algorithm, parser, protocol or serialization contract, or when an edge-level test would be disproportionately costly or diagnostically weak. State the reason for moving inward. Do not choose a boundary merely because it matches a class, method, file, interface, or collaborator.

Place fakes, simulators, and stubs only at genuine process or environmental boundaries, such as a payment provider, time source, external transport, or third-party service. Use them to supply realistic stimuli or capture externally meaningful effects. Do not use mocking frameworks to expose or assert internal call choreography, invocation order, or dependency wiring as a proxy for delivered behavior.

When the repository already provides reliable test-hosted infrastructure for a database, message broker, or another component inside the chosen boundary, prefer that established composition to mocking internal interfaces. This does not require live third-party systems or the largest possible environment.

## Protect behavior, not incidental structure

Do not test private state, internal calls, class or interface shapes, reflection-visible names or attributes, source-file names, folder or module layout, one-type-per-file conventions, incidental serialization, generated markup, or snapshots merely to preserve the current implementation. If a refactor can preserve the externally observable requirement while making the test fail, the test is probably coupled to implementation.

Structure may be tested when it is itself an explicit contract consumed by users, tooling, dependency injection, serialization, plugin discovery, or another external party. Name that contract and, when practical, observe it through the relevant consumer or boundary. Freeze only the schema, name, or shape that the consumer actually depends on.

These examples show the distinction:

```text
Backend — good
Test: Customers_receive_a_receipt_after_payment_is_accepted
Given: A customer has an unpaid order
When: The payment provider reports an accepted payment
Then: A receipt is available to the customer

Backend — coupled
Test: PaymentHandler_calls_receipt_service_after_repository_save
Then: The receipt mock is invoked after the repository mock

Frontend — good
Test: Shoppers_see_when_an_item_becomes_unavailable
Given: A shopper is viewing an available item
When: The server reports that the item is out of stock
Then: The item is shown as unavailable to the shopper

Frontend — coupled
Test: ProductCard_sets_isOutOfStock_and_matches_the_snapshot
Then: Component state and generated markup match the current implementation
```

## Finish each change

Before handoff, confirm:

- [ ] Every agreed acceptance criterion maps to a named test or an intentional non-executable planning item.
- [ ] Non-trivial behavior is understood as Features, Rules, and concrete Examples before test mechanics are selected.
- [ ] Test titles use domain language and state observable behavior, not mechanisms.
- [ ] Behavior-facing scenarios expose meaningful Given/When/Then semantics; `And`, `But`, shared context, and parameterization do not hide distinct behavior.
- [ ] Tests enter through the smallest representative system edge that proves the requirement, unless a stated reason justifies a narrower boundary.
- [ ] Fakes and stubs replace only genuine process boundaries; assertions prove outcomes rather than internal calls.
- [ ] Structural assertions protect an explicit external or tooling contract, not incidental implementation shape.
- [ ] No pending scaffold can silently pass.
