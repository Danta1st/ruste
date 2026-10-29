# C# and NUnit

Read this reference only when the consuming project uses C# and NUnit. Inspect its target framework, package references or central package management, NUnit adapter, analyzer settings, and nearby tests before choosing syntax. Defer to the installed versions and established project conventions when they differ from this guide; the marketplace does not impose a .NET or NUnit version.

## Names and scenario structure

Use a sentence-case C# method identifier with underscores between words. State an action or relevant condition and the observable outcome. Keep fixtures focused on a domain or feature; do not organize them around production methods.

```csharp
[Test]
public async Task Customers_can_cancel_orders_before_fulfilment()
{
    // Given: A customer has an order that has not entered fulfilment
    // When: The customer cancels the order
    // Then: The order is shown as cancelled
    // And: The order is not sent for fulfilment

    await customer.Cancels(order.Id);
    var visibleOrder = await customer.Views(order.Id);

    Assert.That(visibleOrder.Status, Is.EqualTo("Cancelled"));
    Assert.That(fulfilment.ReceivedOrderIds, Does.Not.Contain(order.Id));
}
```

The example omits setup details to keep the naming and scenario shape visible. Here, `customer` is a scenario driver through the representative application edge and `fulfilment` captures an effect at a genuine process boundary; neither wraps a production class for direct invocation. In real tests, make the Given state explicit through the project's fixtures or helpers.

Do not name a test after a production method, route, status code, exception type, persistence operation, or mock verification. Those can be assertion details; they are not the functional requirement.

## NUnit conventions

- Use `[Test]` for a single test and the consuming project's established NUnit idiom for parameterized cases.
- An asynchronous test without an expected return value must return `Task`. Declare a test that awaits operations as `async Task`, not `async void`. Check the installed NUnit version before relying on version-sensitive async setup, teardown, or result behavior.
- Use `[Category("...")]` only when the repository already defines a category taxonomy or its test commands filter by category. Test runners may expose NUnit categories as traits; reuse the repository's taxonomy and filter syntax instead of inventing either for a single change.

## Pending scaffolds

Follow the repository's existing pending-test convention. If it has none, `[Explicit]` with a clear reason can keep an agreed scaffold out of ordinary runs while allowing deliberate execution. A deliberately executed scaffold must fail visibly until implemented:

```csharp
[Test]
[Explicit("Pending: agreed requirement not yet implemented")]
public void Customers_cannot_cancel_orders_after_fulfilment_begins()
{
    // Given: A customer's order has entered fulfilment
    // When: The customer requests cancellation
    // Then: The order remains in fulfilment and cancellation is declined

    Assert.Fail("Pending executable requirement");
}
```

Use `[Ignore("reason")]` only when that is the repository's convention for a temporarily skipped test. Give either attribute a specific reason and remove the pending state when implementing the scenario. Never leave a runnable empty method.

## Integration boundary

Exercise the composed application through the smallest representative entry point that proves the requirement, and keep production components in place unless they cross a genuine process boundary. Scenario drivers may hide transport plumbing, but their operations must represent user or external-system stimuli rather than direct calls to production classes. For ASP.NET Core applications, the installed `Microsoft.AspNetCore.Mvc.Testing` package can provide a test host and server through `WebApplicationFactory<TEntryPoint>`; do not add it or impose that architecture when the repository uses another boundary.

When the repository already supplies reliable test-hosted databases, message brokers, or similar infrastructure within the selected boundary, use that established composition rather than mocking internal application interfaces. This preference does not require live third-party services or justify a larger environment than the behavior needs.

Organize integration tests, shared fixtures, data reset, and categories according to the consuming repository. Assert business outcomes through the public edge and observable state rather than controller, handler, repository, or message-call choreography.

## Current official sources

Consult these after checking installed versions whenever behavior may be version-sensitive:

- [NUnit `Test` attribute](https://docs.nunit.org/articles/nunit/writing-tests/attributes/test.html)
- [NUnit `Category` attribute](https://docs.nunit.org/articles/nunit/writing-tests/attributes/category.html)
- [NUnit `Explicit` attribute](https://docs.nunit.org/articles/nunit/writing-tests/attributes/explicit.html)
- [NUnit `Ignore` attribute](https://docs.nunit.org/articles/nunit/writing-tests/attributes/ignore.html)
- [NUnit analyzer rule for asynchronous test return types](https://docs.nunit.org/articles/nunit-analyzers/NUnit1013.html)
- [ASP.NET Core integration tests](https://learn.microsoft.com/aspnet/core/test/integration-tests)
