# Notes

[Initializing Tests with @Before Methods](https://www.educative.io/courses/unit-testing-java8-junit/initializing-tests-with-before-methods)

If two tests have the same setup logic, we can move that common logic into a `@Before` method. This way, JUnit will run the code in any `@Before` methods before each test.

---
[Hamcrest Assertions](https://www.educative.io/courses/unit-testing-java8-junit/hamcrest-assertions)

```java
// Instead of assertTrue(value > 0), we use:
assertThat(value, greaterThan(0));
```

- Easier to read, more natural language  
- Supports more flexible matchers  
- Alternative: AssertJ  

---
[Testing the Expected Exceptions](https://www.educative.io/courses/unit-testing-java8-junit/testing-the-expected-exceptions)

```java
@Test(expected = InsufficientFundsException.class)
public void throwsWhenWithdrawingTooMuch() {
    Account account = new Account(100);
    account.withdraw(200);
}

void withdraw(int dollars) {
    if (balance < dollars)
        throw new InsufficientFundsException("balance only " + balance);
    balance -= dollars;
}
```

[Assert an Exception Is Thrown in JUnit 4 and 5 | Baeldung](https://www.baeldung.com/junit-assert-exception) (new API)

---
**Common Mistakes**  

When we write tests, focus on the behaviors of our class, not on testing the individual methods.

For example, writing a test just to check getBalance(). This does not add much value because the method may only return a variable.

**Better Approach**  

Instead, we should test the behavior of the ATM class, such as:  

- **Single deposit** (`makeSingleDeposit`)  
- **Multiple deposits** (`makeMultipleDeposits`)  

Each test will use `getBalance()` to verify the result, but they do not focus on testing `getBalance()` itself.

---
```java
@Test
public void matches() {
  Profile profile = new Profile("Bull Hockey, Inc.");
  Question question = new BooleanQuestion(1, "Got milk?");

  // Answers false when must-match criteria is not met
  profile.add(new Answer(question, Bool.FALSE));
  Criteria criteria = new Criteria();
  criteria.add(
    new Criterion(new Answer(question, Bool.TRUE), Weight.MustMatch));

  assertFalse(profile.matches(criteria));

  // Answers true for any don't-care criteria  
  profile.add(new Answer(question, Bool.FALSE));
  criteria = new Criteria();
  criteria.add(
    new Criterion(new Answer(question, Bool.TRUE), Weight.DontCare));

  assertTrue(profile.matches(criteria));
}
```

Although this approach reduces code duplication (by sharing data setup), it has **major drawbacks**:  

- If one `assertion` fails, the following tests won’t run.  
- If the test fails, it’s hard to know the exact reason because only `matches()` fails.  
- Tests lose independence, which can cause failure to spread.  

```java
// Test 1 (answers false when must-match criteria is not met)
@Test

// Test 2 (answers true for any don't-care criteria)
@Test
```

---
**Naming**  

**Action → Result** (`doingSomeOperationGeneratesSomeResult`)  

- `withdrawingFundsReducesBalance()`  
- `depositingMoneyIncreasesBalance()`  
- `enteringInvalidPasswordTriggersError()`  

---
```java
@Test
@Ignore("don't forget me!")
public void somethingWeCannotHandleRightNow() {
// ...
}
```

---
Use Mock Objects (like Mockito).  

Remove try/catch blocks; declare exceptions in the test method instead.  

# pragmatic-unit-testing-in-java-8-with-junit
  Sample codes of the book "Pragmatic Unit Testing in Java 8 with JUnit" (https://pragprog.com/book/utj2/pragmatic-unit-testing-in-java-8-with-junit)
  
  
  Samples of the book were arranged into a maven java project, with multiple modules. Modules refer to content of the book as:

    Chapter 1: module 1
    
    Chapter 2: modules 6, 9
    
	Chapter 3, Chapter 4: module 13

    Chapter 5: 16

    Chapter 6, 7: modules 14, 15, 16
	
    Chapter 8: modules 16, 17, 18, 19, 20, 20-mis, 22, 23
	
    Chapter 9: modules big-1, big-2, big-3, big-4, big-5, big-6

  All test can be run with the command at root folder (require maven installed): mvn test
	