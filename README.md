# Notes

[Initializing Tests with @Before Methods](https://www.educative.io/courses/unit-testing-java8-junit/initializing-tests-with-before-methods)

> If both tests have the same logic, we can move that common logic into an `@Before` method. That way, each JUnit first executes code in any methods marked with the `@Before` annotation.

---
[Hamcrest Assertions](https://www.educative.io/courses/unit-testing-java8-junit/hamcrest-assertions)

```java
// Thay vì assertTrue(value > 0), ta dùng: 
assertThat(value, greaterThan(0));
```

Dễ đọc, gần với ngôn ngữ tự nhiên

Hỗ trợ nhiều matcher linh hoạt hơn

alternatives: assertj

---
[Testing the Expected Exceptions](https://www.educative.io/courses/unit-testing-java8-junit/testing-the-expected-exceptions)

```java
@Test(expected = InsufficientFundsException.class)
public void throwsWhenWithdrawingTooMuch() {
    Account account = new Account(100);
    account.withdraw(200); // Số tiền rút lớn hơn số dư
}

void withdraw(int dollars) {
    if (balance < dollars)
        throw new InsufficientFundsException("balance only " + balance);
    balance -= dollars;
}
```

[Assert an Exception Is Thrown in JUnit 4 and 5 | Baeldung](https://www.baeldung.com/junit-assert-exception) new api

---
**Sai lầm phổ biến**

Một sai lầm thường gặp là tập trung kiểm thử từng phương thức một cách riêng lẻ, chẳng hạn như viết một bài kiểm thử chỉ để xác nhận `getBalance()`. Tuy nhiên, điều này không mang nhiều ý nghĩa vì phương thức này có thể chỉ đơn giản là trả về giá trị của một biến.

**Hướng tiếp cận đúng**

Thay vào đó, chúng ta nên kiểm thử các hành vi của lớp ATM, ví dụ như:

- **Gửi tiền một lần** (`makeSingleDeposit`)
- **Gửi tiền nhiều lần** (`makeMultipleDeposits`)

Mỗi bài kiểm thử này sẽ sử dụng `getBalance()` để xác minh kết quả, nhưng chúng không tập trung vào kiểm thử bản thân `getBalance()`.

---
```java
@Test
public void matches() {
  Profile profile = new Profile("Bull Hockey, Inc.");
  Question question = new BooleanQuestion(1, "Got milk?");

  // answers false when must-match criteria not met
  profile.add(new Answer(question, Bool.FALSE));
  Criteria criteria = new Criteria();
  criteria.add(
    new Criterion(new Answer(question, Bool.TRUE), Weight.MustMatch));

  assertFalse(profile.matches(criteria));

  // answers true for any don't care criteria 
  profile.add(new Answer(question, Bool.FALSE));
  criteria = new Criteria();
  criteria.add(
    new Criterion(new Answer(question, Bool.TRUE), Weight.DontCare));

  assertTrue(profile.matches(criteria));
}
```

Mặc dù cách tiếp cận này giúp giảm lặp lại mã (vì dùng chung setup dữ liệu), nhưng nó có **nhược điểm lớn**:

- Khi một `assertion` thất bại, các kiểm thử sau đó sẽ không được thực thi.
- Khi test thất bại, ta không biết ngay nguyên nhân cụ thể, vì chỉ có một phương thức `matches()` bị lỗi.
- Mất đi tính độc lập của từng kiểm thử, có thể dẫn đến việc lỗi lan truyền.

```java
// Test 1 (answers false when must-match criteria not met)
@Test

// Test 2 (answers true for any don't care criteria)
@Test
```

---
**Naming techniques**

**Hành động → Kết quả** (`doingSomeOperationGeneratesSomeResult`)

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
	