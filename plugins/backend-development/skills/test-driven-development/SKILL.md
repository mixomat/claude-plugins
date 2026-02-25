---
name: test-driven-development
description: Write unit test first before writing new production code, watch the unit test fail, then write the minimal production code to pass the test. Use when the user asks to implement/add/create/build features or functionality, fix/resolve/patch bugs or issues, refactor/restructure/reorganize code, modify/update/change existing behavior, add new methods/functions/classes, or make code changes. Activate for substantial changes that affect behavior, not formatting or comments.
---

# Test-Driven Development (TDD)

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete

## Red-Green-Refactor

### RED - Write Failing Test

Write one minimal test showing what should happen.

<Good>
```kotlin
@Test
fun `should apply 10 percent discount when order total exceeds 100`() {
  val order = Order(items = listOf(Item("Laptop", 120.0)))

  val discountedTotal = calculateTotal(order)

  discountedTotal shouldBe 108.0
}
```
Clear name, tests real behavior, one thing
</Good>

<Bad>
```kotlin
@Test
fun `test discount`() {
  val mockCalculator = mockk<PriceCalculator>()
  every { mockCalculator.calculate(any()) } returns 108.0

  val result = mockCalculator.calculate(Order(emptyList()))

  verify { mockCalculator.calculate(any()) }
}
```
Vague name, tests mock not code
</Bad>

**Requirements:**
- One behavior
- Clear name
- Real code (no mocks unless unavoidable)

### Verify RED - Watch It Fail

**MANDATORY. Never skip.**

```bash
./gradlew test --tests "path.to.TestClass"
```

Confirm:
- Test fails (not errors)
- Failure message is expected
- Fails because feature missing (not typos)

**Test passes?** You're testing existing behavior. Fix test.

**Test errors?** Fix error, re-run until it fails correctly.

### GREEN - Minimal Code

Write simplest code to pass the test.

<Good>
```kotlin
data class Order(val items: List<Item>)
data class Item(val name: String, val price: Double)

fun calculateTotal(order: Order): Double {
  val subtotal = order.items.sumOf { it.price }
  return if (subtotal > 100) subtotal * 0.9 else subtotal
}
```
Just enough to pass
</Good>

<Bad>
```kotlin
fun calculateTotal(
  order: Order,
  discountRules: List<DiscountRule> = defaultRules,
  taxStrategy: TaxStrategy = DefaultTaxStrategy(),
  currencyConverter: CurrencyConverter? = null
): Money {
  // YAGNI - over-engineered
}
```
Over-engineered
</Bad>

Don't add features, refactor other code, or "improve" beyond the test.

### Verify GREEN - Watch It Pass

**MANDATORY.**

```bash
./gradlew test --tests "path.to.TestClass"
```

Confirm:
- Test passes
- Other tests still pass
- Output pristine (no errors, warnings)

**Test fails?** Fix code, not test.

**Other tests fail?** Fix now.

### REFACTOR - Clean Up

After green only:
- Remove duplication
- Improve names
- Extract helpers

Keep tests green. Don't add behavior.

### Repeat

Next failing test for next feature.

## Red Flags -> Start Over

| Flag | Reality |
|--------|---------|
| "Test passes immediately" | You're testing existing behavior, not new feature |
| "I'll test after the code" | Tests passing immediately prove nothing |
| "Already manually tested" | Ad-hoc ≠ systematic. No record, can't re-run |
| "Test hard = design unclear" | Listen to test. Hard to test = hard to use |

## Example: Bug Fix

**Bug:** Empty email accepted

**RED**
```kotlin
@Test
fun `should reject empty email`() {
  val result = submitForm(FormData(email = ""))

  result.error shouldBe "Email required"
}
```

**Verify RED**
```bash
$ ./gradlew test
FAIL: expected "Email required", got null
```

**GREEN**
```kotlin
data class FormData(val email: String?)
data class FormResult(val error: String? = null, val success: Boolean = false)

fun submitForm(data: FormData): FormResult {
  if (data.email?.trim().isNullOrEmpty()) {
    return FormResult(error = "Email required")
  }
  return FormResult(success = true)
}
```

**Verify GREEN**
```bash
$ ./gradlew test
PASS
```

**REFACTOR**
Extract validation for multiple fields if needed.

## Verification Checklist

- [ ] Every new function/method has a test
- [ ] Watched each test fail before implementing
- [ ] Each test failed for expected reason (feature missing, not typo)
- [ ] Wrote minimal code to pass each test
- [ ] All tests pass
- [ ] Output pristine (no errors, warnings)
- [ ] Tests use real code (mocks only if unavoidable)
- [ ] Edge cases and errors covered

Can't check all boxes? You skipped TDD. Start over.

## When Stuck

| Problem | Solution |
|---------|----------|
| Don't know how to test | Write wished-for API first. Write assertion first. Ask human partner. |
| Test too complicated | Design too complicated. Simplify interface. |
| Must mock everything | Code too coupled. Use dependency injection. |
| Test setup huge | Extract helpers. Still complex? Simplify design. |
| Bug found | Write failing test reproducing it. Follow TDD cycle. |

