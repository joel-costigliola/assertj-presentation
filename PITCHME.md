## AssertJ
##### <span style="font-family:Helvetica Neue; font-weight:bold">Rich and easy to use assertions</span>

http://joel-costigliola.github.io/assertj

---

## Agenda

- What is AssertJ ? |
- Quick start |
- Assertions quick tour |
- Soft assertions |
- Using conditions |
- AssertJ extensions |
- Advanced stuff |
- Hands on ! |

---

## What is AssertJ ?

AssertJ is an assertion java library which:
- provides rich assertions for common java types |
- improves test code readability with its fluent API | 
- easy to use with your favorite IDE |
- reports helpful error messages when assertions fail | 
- has javadoc with code examples |

---

## Quick start

Add assertj-core library to your test dependencies 

![Press Down Key](assets/down-arrow.png)

+++

#### PB2 Test project setup

Adding AssertJ Core to a PB2 Test project:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<ivy-module version="1.0">
  <info module="my.unit.tests" organisation="orchestral" status="integration"></info>
  <configurations>
    <conf name="test"/>
  </configurations>
  <publications>
  </publications>
  <dependencies>
    <dependency conf="test->nodist" name="assertj-core" 
                org="assertj" rev="3.8.0"/>
    <!-- other usual test dependencies -->
    <dependency conf="test->nodist" org="bundle" name="org.junit" rev="4.12.0"/>
    <dependency conf="test->nodist" org="bundle" name="org.mockito" rev="1.9.5"/>
  </dependencies>
</ivy-module>
```

@[5](ivy test config)
@[10-11](assertj-core dependency)

+++

#### Maven or Gradle setup

Maven

```xml
<dependency>
  <groupId>org.assertj</groupId>
  <artifactId>assertj-core</artifactId>
  <version>3.8.0</version>
  <scope>test</scope>
</dependency>
```

Gradle
```
testCompile 'org.assertj:assertj-core:3.8.0'
```

---

## Basic assertions

- Start with Assertions.assertThat(stuffToTest) |
- Discover assertions with code completion | 
- Chain assertions |
- Quick demo |

Note:
Demo: 
* no more confusion about expected vs actual
* assertion description as()
* show representation ?
* IDE config to get assertThat directly
* BDDAssertions for then
* implement WithAssertions 

+++

#### Examples

Basic assertions example:

```java
assertThat("Gandalf")                      
    .as("Check a famous magician name")
    .isNotNull()                       
    .isInstanceOf(String.class)        
    .isNotEqualTo("Saroumane")         
    .isEqualTo("Gandalf");             
```

@[1](`import static org.assertj.core.api.Assertions.assertThat;`)
@[2](describe your assertion (to ease understand the error))
@[3-6](chain assertions)

---

## Collection assertions

- Works for Iterable, arrays and Stream |
- Different "contains" assertion flavors |
- feature highlight: extracting |
- feature highlight: filter | 

Note:
* Stream are converted to List to allow multiple assertions since you only consume a Stream once.
* javadoc example: http://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractIterableAssert.html#containsExactly-ELEMENT...-
* collections formatting goodies when too many items

+++

#### Examples

```java
Iterable<Ring> elvesRings = asList(vilya, nenya, narya);
                                                                    
assertThat(elvesRings)                                              
    .isNotEmpty()                                               
    .hasSize(3)                                                 
    .contains(nenya, narya)                                            
    .doesNotContain(oneRing)                                    
    // order does not matters                                   
    .containsOnly(nenya, vilya, narya)                          
    // order matters!
    .containsExactly(vilya, nenya, narya);                      
```

@[1-3, 6](*contains* checks if the given elements are in the iterable)
@[1-3, 8-9](*containsOnly* expects all the elements)
@[1-3, 10-11](*containsExactly* expects all the elements in the correct order)

+++

#### Extracting feature

```java
// TolkienCharacter fields: name, age, Race
// Race field: name
frodo = new TolkienCharacter("Frodo", 33, HOBBIT);
...
boromir = new TolkienCharacter("Boromir", 37, MAN);

fellowshipOfTheRing = asList(frodo, sam, merry, pippin, gandalf,
                             legolas, gimli, aragorn, boromir);

assertThat(fellowshipOfTheRing)                       
    .extracting("name") // or use lambda: tc -> tc.getName()
    .contains("Boromir", "Gandalf", "Frodo", "Legolas")
    .doesNotContain("Sauron", "Elrond");               
```
@[1-2](simple data classes)
@[3-8](init a list of famous LotR characters)
@[10-13](let's check the names of the fellowshipOfTheRing characters)
@[10-11](create a new List to test with names of fellowshipOfTheRing characters)
@[12-13](assertions on the extracted names)

+++

#### Filter feature

```java
assertThat(fellowshipOfTheRing)                                 
    .filteredOn(tc -> tc.getRace().equals(HOBBIT))               
    .containsOnly(sam, frodo, pippin, merry);

assertThat(this.fellowshipOfTheRing)                     
    .filteredOn(tc -> tc.getRace().equals(HOBBIT))   
    .extracting(tc -> tc.getName())                  
    .containsOnly("Sam", "Frodo", "Pippin", "Merry");    
```
@[1-2](use of a Java 8 *Predicate* to filter fellowshipOfTheRing)
@[1-3](check it contains only Hobbits characters)
@[5-7](combine filter and extracting FTW !)
@[5-8](check it contains the expected Hobbit's names)

---

## Exception assertions

- Old school way of testing exceptions |
- Better way with assertThatThrownBy |
- Better way with assertThatExceptionOfType  |
- Better way with catchThrowable  |

Note:
* JUnit Rule: bad because it puts the assertions before the code
* assertThatCode(() -> {}).doesNotThrowAnyException();

+++

#### Basic exception testing (1)

```java
void boom() {
  Exception cause = new Exception("let's push that red button");
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
@[1-4](let's test this method)
@[6-9, 13](catch the exception and test it)
@[9-11](some of the exception assertions)
@[8-14](make the test fail if no exception was thrown)

+++

#### Better exception testing (2)

Use *assertThatThrownBy* 

```java
assertThatThrownBy(() -> boom())
    .isInstanceOf(IllegalStateException.class)
    .hasMessage("%sm!", "boo")
    .hasCauseInstanceOf(RuntimeException.class)
    .hasStackTraceContaining("let's push that red button");
```
@[1](Use a lambda to pass the code to test)
@[2-5](chain exception assertions)

+++

#### Better exception testing (3)

Use *assertThatExceptionOfType* 

```java
assertThatExceptionOfType(IllegalStateException.class)
    .isThrownBy(() -> boom())
    .withMessage("boom!")
    .withCauseInstanceOf(RuntimeException.class);

assertThatIllegalStateException()
    .isThrownBy(() -> boom())
    .withCauseInstanceOf(RuntimeException.class)
    .withMessageContaining("boo");
```
@[1](Pass the expected exception type)
@[1-2](Use a lambda to pass the code to test)
@[1-4](chain exception assertions)
@[6-7](common exceptions comes with this pattern)
@[6-9](usual assertions chaining)

+++

#### BDD exception testing BDD (4)

Use *catchThrowable* 

```java
// GIVEN
ThrowingCallable codeCall = () -> boom();
// WHEN
final Throwable thrown = catchThrowable(codeCall);
// THEN
assertThat(thrown)
    .isInstanceOf(IllegalStateException.class)
    .hasMessage("boom!")
    .hasNoSuppressedExceptions();
```
@[1-2](GIVEN some code)
@[3-4](WHEN calling the code)
@[5-9](THEN check the caught exception is)


---

## Soft assertions

- Record all assertion errors (don't fail at the first one) |
- Easy soft assertions with JUnitSoftAssertions rule |
- BDD flavor with JUnitBDDSoftAssertions |

Note:
Demo: 
* BDD soft assertions

+++

#### Soft assertions example

```java
final SoftAssertions softly = new SoftAssertions();

softly.assertThat("Michael Jordan - Bulls")
          .startsWith("Mike")
          .contains("Lakers")
          .endsWith("Chicago");

// don't forget to call assertAll() !
softly.assertAll();
```

@[1](we need a SoftAssertions wrapper)
@[3-6](record potential assertion failures)
@[8-9](report assertion failures if any)

+++

#### Assertions errors report

```
The following 3 assertions failed:
1) 
Expecting:
 <"Michael Jordan - Bulls">
to start with:
 <"Mike">

2) 
Expecting:
 <"Michael Jordan - Bulls">
to contain:
 <"Lakers"> 
3) 
Expecting:
 <"Michael Jordan - Bulls">
to end with:
 <"Chicago">
```

@[1](number of failures)
@[2-6](first assertion error)
@[8-12](second assertion error)
@[13-17](last assertion error)

+++

#### JUnit Soft assertions example

```java
@Rule
public JUnitSoftAssertions softly = new JUnitSoftAssertions();

@Test
public void junit_soft_assertions_example() {

  softly.assertThat("Michael Jordan - Bulls")
            .startsWith("Mike")
            .contains("Lakers")
            .endsWith("Chicago");

  // no need to call assertAll() ! :)
}
```

@[1-2](init the JUnit rule)
@[4-10, 13](use soft assertions as usual)
@[1-2, 12](the rule calls assertAll() for us)

---

## Conditions

- Extend AssertJ assertions with conditions |
- Similar to Hamcrest matchers |
- Combine conditions |
- Using Condition with collections |

Note:
* Stream are converted to List to allow multiple assertions since you only consume a Stream once.

+++

#### Examples

```java
List<String> jedis = asList("Luke", "Yoda", "Obiwan");
List<String> siths = asList("Sidious", "Vader", "Plagueis");
// build condition with a Predicate and a description
Condition<String> jedi = 
        new Condition<>(jedis::contains, "jedi");
Condition<String> sith = 
        new Condition<>(siths::contains, "sith");

assertThat("Yoda").is(jedi);
assertThat("Vader").is(sith).isNot(jedi);

// 'has(condition)' is an alias of 'is(condition)'
Condition<String> jediPowers = new Condition<>(jedis::contains, "jedi powers");
assertThat("Yoda").has(jediPowers);
assertThat("Sponge Bob").doesNotHave(jediPowers);
```

@[1-7](create condition with a Predicate)
@[9-10](assertions using conditions)
@[12-15](*has* is an alias of *is* for readability)

+++

#### Combining conditions

AssertJ provides operators to combine conditions:
- *allOf* : all conditions must be met  
- *anyOf* : at leats one of the conditions must be met 
- *not* : the condition must not be met 
<br>
```java
assertThat("Vader").is(anyOf(jedi, sith))
                   .is(not(jedi));
```

+++

#### Using Condition with collections

```java
assertThat(asList("Luke", "Yoda")).are(jedi);
assertThat(asList("Luke", "Yoda")).have(jediPowers);

assertThat(asList("Chewbacca", "Solo")).areNot(jedi);
assertThat(asList("Chewbacca", "Solo")).doNotHave(jediPowers);

List<String> characters = asList("Luke", "Yoda", "Chewbacca");
assertThat(characters).areAtLeast(2, jedi);
assertThat(characters).haveAtLeast(2, jediPowers);
assertThat(characters).areAtMost(2, jedi);
assertThat(characters).haveAtMost(2, jediPowers);
assertThat(characters).areExactly(2, jedi);
assertThat(characters).haveExactly(2, jediPowers);
```
@[1-2](check that all elements meet the condition)
@[4-5](check that **no** elements meet the condition)
@[7-9](check that at least 2 elements meet the condition)
@[7, 10-11](check that at most 2 elements meet the condition)
@[7, 12-13](check that exactly 2 elements meet the condition)

+++


---
## Advanced assertions

- Use satisfies to group logical assertions |
- Use matches to check a logical condition |
- Using comparators |
- Using field by field comparison |

Note:
Demo: 
* no more confusion about expected vs actual
* assertion description as()
* chaining assertion
* show representation ?

---


