---
marp: true
theme: default
---

## Tests in java with AssertJ

https://assertj.github.io/doc/


---

## Agenda

- What is AssertJ ?
- Assertions tour
- Soft assertions
- Assumptions
- Comparators and recursive comparison assertions
- Custom assertions
- Different test techniques overview

---

## What is AssertJ ?

AssertJ is an assertion java library which:
- provides rich assertions (more than 1000) for 50+ java types
- improves test code readability with its fluent API
- easy to use with your IDE code completion (unlike Hamcrest)
- reports helpful assertions error messages
- has javadoc with code examples
- has modules for Guava, Relational DB and Neo4J
- mentioned in Thoughtworks tech radar

---

## AssertJ philosophy

- Community driven, 265 contributors
- Try to give a good experience to first time contributors
  - Fast feedback (challenging at time)
  - Mentoring for junior contributors
  - Learn to say no at time but always explain why not to make it personal
- For me, a social experiment on how to manage a community
- API is huge (maybe bloated) but it's kinda by design

---

## Basic assertions

- Start with `Assertions.assertThat(objectUnderTest)`
- Discover assertions with code completion
- Chain as many assertions as you need

```java
import static org.assertj.core.api.Assertions.assertThat;

assertThat("Gandalf")
    .as("Check a famous magician name") // describe assertion
    .isNotNull() // generic object assertion
    .isInstanceOf(String.class)
    .isNotEqualTo("Saroumane")
    .startsWith("Gand"); // string assertion
```

---

## Collection assertions

Work for iterables, arrays and streams

Examples:
```java
Iterable<Ring> elvesRings = asList(vilya, nenya, narya);

assertThat(elvesRings).isNotEmpty()
                      .hasSize(3)
                      .contains(nenya, narya)
                      .doesNotContain(oneRing)
                      // order does not matters nor duplicates
                      .containsOnly(nenya, vilya, narya)
                      // order and duplicates matter
                      .containsExactly(vilya, nenya, narya);
```

<!--
* Stream are converted to List to allow multiple assertions since you only consume a Stream once.
* javadoc example: http://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractIterableAssert.html#containsExactly-ELEMENT...-
* collections formatting goodies when too many items
-->

---

#### Extracting API

Like `Stream.map` but available for iterables and array too

```java
// TolkienCharacter fields: name, age, Race
frodo = new TolkienCharacter("Frodo", 33, HOBBIT);
...
boromir = new TolkienCharacter("Boromir", 37, MAN);

fellowshipOfTheRing= list(frodo, sam, merry, pippin, gandalf,
                          legolas, gimli, aragorn, boromir);

assertThat(fellowshipOfTheRing)
    .extracting(TolkienCharacter::getName) // or use "name" (less type safe)
    .contains("Boromir", "Gandalf", "Frodo", "Legolas")
    .doesNotContain("Sauron", "Elrond");
```

Note: `flatExtracting`/`flatMap` are also available.

---

#### Filter API

Like `Stream.filter` but available for iterables and array too


```java
assertThat(fellowshipOfTheRing)
    .filteredOn(tc -> tc.getRace().equals(HOBBIT))
    .containsOnly(sam, frodo, pippin, merry);
```

Combine extracting and filter for the win:
```java
assertThat(fellowshipOfTheRing)
    .filteredOn(TolkienCharacter::getRace, HOBBIT)
    .extracting(TolkienCharacter::getName)
    .containsOnly("Sam", "Frodo", "Pippin", "Merry");
```

<!-- use of a Java 8 *Predicate* to filter fellowshipOfTheRing -->

---

## Asserting exceptions

- Old school way of testing exceptions
- Better way with assertThatThrownBy
- Better way with assertThatExceptionOfType
- Better way with catchThrowable

<!--
* there is a better way / there are better ways!
* JUnit Rule: bad because it puts the assertions before the code
* assertThatCode(() -> {}).doesNotThrowAnyException();
 -->
---

#### Exception testing: old way (don't do this)

```java
void boom() {
  Exception cause = new RuntimeException("red button pushed");
  throw new IllegalStateException("boom!", cause);
}
// Old school way of testing code throwing exceptions
try {
  boom();
} catch (final Exception exception) {
  assertThat(exception)
      .isInstanceOf(IllegalStateException.class)
      .hasMessage("boom!");
  return;
}
fail("Should not arrive here");
```
<!--
make the test fail if no exception was thrown
-->
---

#### Better exception testing: assertThatThrownBy

```java
import static org.assertj.core.api.Assertions.assertThatThrownBy;

assertThatThrownBy(() -> boom())
    .isInstanceOf(IllegalStateException.class)
    .hasMessage("%sm!", "boo")
    .hasCauseInstanceOf(RuntimeException.class)
    .hasStackTraceContaining("red button");
```

---

#### Better exception testing: assertThatExceptionOfType

```java
import static org.assertj.core.api.Assertions.assertThatExceptionOfType;

assertThatExceptionOfType(IllegalStateException.class)
    .isThrownBy(() -> boom())
    .withMessage("boom!")
    .withCauseInstanceOf(RuntimeException.class);
```

Common exception shortcuts:
- `assertThatIllegalStateException()`
- `assertThatIllegalArgumentException()`
- `assertThatIOException()`
- `assertThatNullPointerException()`

---

#### Better exception testing: catchThrowable

```java
import static org.assertj.core.api.Assertions.catchThrowable;
import static org.assertj.core.api.BDDAssertions.then;

// WHEN
final Throwable thrown = catchThrowable(() -> boom());

// THEN
then(thrown).isInstanceOf(IllegalStateException.class)
            .hasMessage("boom!")
            .getCause()
            .hasMessage("red button pushed");
```

Use `catchThrowableOfType(() -> boom(), IllegalStateException.class)` to directly check the exception type.

---

## Soft assertions

- Record all assertion errors (don't fail at the first one)
- Integration with JUnit4 and JUnit5
- BDD flavor with JUnitBDDSoftAssertions

```java
final SoftAssertions softly = new SoftAssertions();

softly.assertThat("Michael Jordan - Bulls")
          .startsWith("Mike")
          .contains("Lakers")
          .endsWith("Chicago");

// don't forget to call assertAll() !
softly.assertAll();
```

---

#### Soft assertions errors report

```text
Multiple Failures (3):
-- failure 1 --
Expecting actual:
  "Michael Jordan - Bulls"
to start with:
  "Mike"
-- failure 2 --
Expecting actual:
  "Michael Jordan - Bulls"
to contain:
  "Lakers"
-- failure 3 --
Expecting actual:
  "Michael Jordan - Bulls"
to end with:
  "Chicago"
```

---

#### JUnit Soft assertions example

```java
@ExtendWith(SoftAssertionsExtension.class)
class JUnit5SoftAssertionsExtensionAssertionsExamples {

  @InjectSoftAssertions
  JUnitSoftAssertions softly;

  @Test
  void junit_soft_assertions_example() {

    softly.assertThat("Michael Jordan - Bulls")
              .startsWith("Mike")
              .contains("Lakers")
              .endsWith("Chicago");

    // no need to call assertAll() ! :)
  }
}
```

---

## Assumptions

- Allows to skip tests when assumptions are not met
- The assumption API is similar to the assertion API (thanks to proxying magic)

```java
import static org.assertj.core.api.Assumptions.assumeThat;

@Test
void should_be_skipped_as_assumption_is_not_met() {
  assumeThat(fellowshipOfTheRing).contains(sauron);
  // never executed, the whole test is skipped!
  assertThat(true).isFalse();
}

@Test
void should_be_executed_as_assumption_is_met() {
  assumeThat(fellowshipOfTheRing).contains(frodo);
  // executed, can succeed or fail like any assertions
  assertThat(frodo.getRace()).isEqualTo(HOBBIT);
}
```

---

## Using comparators

Specify your own comparator when evaluating assertions.

```java
// standard comparison: obviously fails!
assertThat(frodo).isNotEqualTo(sam);
assertThat(fellowshipOfTheRing).contains(sauron);

Comparator<TolkienCharacter> byRace = (t1, t2) -> t1.race.compareTo(t2.race);

// succeeds as we compare character's race only
assertThat(frodo).usingComparator(byRace)
                 .isEqualTo(sam)
                 .isIn(aragorn, gandalf, sam);

// succeeds because Sauron is a Maia like Gandalf
assertThat(fellowshipOfTheRing).usingElementComparator(byRace)
                               .contains(gandalf, sauron);
```

---

#### Recursive comparison assertion

Useful when:
- comparing data classes not overriding *equals*
- supports properties and private fields
- comparison is configurable:
  - ignore fields, types, null fields
  - specify fields or type comparator
  - ...

https://assertj.github.io/doc/#assertj-core-recursive-comparison

---

## Custom assertions

Express assertions in the domain language:
- great to express the domain through tests (DDD), error messages included
- make the assertions code more readable
- use assertion generator to generate assertions based on class properties
- generated assertions are not perfect but a nice starting point

```java
frodo = new TolkienCharacter("Frodo", 33, HOBBIT);
// domain assertions:
assertThat(frodo).hasName("Frodo")
                 .hasAge(33)
                 .hasRace(Race.HOBBIT);
```
<!--
Note:
- error message mention the name
 -->

---

## Unit tests good practices

- test one thing per test case
- prefer BDD style for clearly separating test steps: Given / When / Then
- write tests as a specification (JUnit `@DisplayName` gives a good output)
- use parameterized tests to avoid repeating yourself
  - parameterizing both input and expected ouput is a good technique
- mocks: good for external services but make refactoring harder (lot of tests rewrite)

---

## Mutation testing vs test coverage 1/2

Code to test
```java
boolean strictlyGreaterThanTen(int x) {
  if(x >= 10) return true; // oops! should be > 10
  else return false;
}
```

Unit tests
```java
@Test
boolean myTest() {
  assertThat(strictlyGreaterThanTen(11)).as("check with 11").isTrue();
  assertThat(strictlyGreaterThanTen(9)).as("check with 9").isFalse();
}
```

---

## Mutation testing vs test coverage 2/2

The code has 100% test coverage but still has a bug as `10` (boundary) is not tested
```java
assertFalse(strictlyGreaterThanTen(10)); // should pass but fail
```

Mutation testing:
- introduce fault in the code (mutation)
- expect a test to fail for each fault
- surviving mutations (no tests failed) indicate untested code
- running mutation testing is expensive
- https://pitest.org/ is a nice tool for Java

---

## Property based testing (PBT)

- enforce program invariants in tests
- run tests against data generated by the PBT tool
- a good PBT tool reduces the failing data to its simplest form
- https://jqwik.net/

---

## "Big" tests

- integration, functional and end to end testing
- contract testing for APIs (https://docs.pact.io/)
- chaos testing: introduce random faults to verify the resilience of the system
- A/B testing & tests in production
  - test the real stuff, preprod is not prod: different network, config, amount of data,...
  - rely on solid metrics, dashboard and alerts
  - require to easily be able to rollback to a previous working version

---

## Final words

- Use AssertJ or any fluent api to make code more readable
- Contribute to open source, don't just use it
- Use different testing techniques
