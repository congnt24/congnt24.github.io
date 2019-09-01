---
layout: post
title: "Test: Unit Test Java app with junit, mockito"
categories: test
image: "https://miro.medium.com/max/1200/1*3NDVbzYlOTLyRSrpay9uYw.png"
tags: [backend, java, test, unit, integrate]
---
A brief overview about using junut5 and mockito to weite unit test.

## 1. Junit overview
- A test is the validation of functional and nonfunctional requirements before it is shipped to a customer.
- Unit testing code means validation or performing the sanity check of code. Sanity check is a basic test to quickly evaluate whether the result of a calculation can possibly be true. It is a simple check to see whether the produced material is coherent.
- Unit testing is a common practice in test-driven development (TDD).
- Java code can be unit tested using a code-driven unit testing framework.
- Few of the available code-driven unit testing frameworks for Java:
  * SpryTest
  * Jtest
  * JUnit
  * TestNG
  * JUnit is the most popular and widely used unit testing framework for Java.
- JUnit is an annotation-based.
## 2. Dependencies

```yaml
<!--Junit5-->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.5.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-params</artifactId>
    <version>5.5.1</version>
    <scope>test</scope>
</dependency>
<!--Mockito-->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>2.28.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>2.28.2</version>
    <scope>test</scope>
</dependency>
```

## 3. Cheat sheet
### 3.1 Common annotation
- @Test: test public method
- @BeforeEach: Run before every test
- @AfterEach: Run after every test
- @BeforeAll, @AfterAll: Apply for public static method, execute before/after the first/last test

### 3.2 Verify test conditions: Assert & Assume
- Assert.assertTrue("failure message", 100 > 0); in JUnit 5, failure message is the last param
- Assert.assertNull()
- Assert.assertNotNull()
- Assert.assertEquals: Compare value: === string.equals()
- Assert.assertSame: compare object: ==
- Assert.assertNotSame: !=
- AssertThat(Object actual, Matcher matcher): More powerful assert
  Some default matcher:
  - equalTo()
  - Is()
  - Not()
  - Either().or()
  - Both().and()
  - anyOf()
  - List: hasItem, hasItems
  - String: startWith, endWith, containsString
- To test Exception:
@Test(expected=RuntimeException.class): handle exception
//Assume
- Assume.assumeFalse(isSonarRunning); If assume return true, test method will be execute, if not test method will be ignore

### 3.3 Parameterized Test
In junit 5, using @ParameterizeTest instead of @Test. It will run your test method many times like below:
```java
@ParameterizedTest
@CsvSource({
    "1, 2",
    "2, 4",
    "3, 6",
    "4, 8"
})
public void multiple2(int number, int expected) {
    assertEquals(expected, 2 * number)
}
```
Above test will run 5 times with passed parameters and expect result.

### 3.4 Ordering
We can define which test run first, second... in junit5 like below:
```java
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
public class KafkaTests {
    @Test
    @Order(1)
    public void runFirst() {
        assertTrue(true);
    }
    
    @Test
    @Order(2)
    public void runSecond() {
        assertTrue(true);
    }
}
```
- First we have to add @TestMethodOrder annotation to the class
- Then, define your methods order by using @Order(number)

### 4. Mockito
Mockito is a mocking framework, JAVA-based library that is used for effective unit testing of JAVA applications. Mockito is used to mock interfaces so that a dummy functionality can be added to a mock interface that can be used in unit testing.
Here are some example of using mockito with junit 5:

```java
@ExtendWith(MockitoExtension.class)
@MockitoSettings(strictness = Strictness.LENIENT)
public class KafkaTests {
    @Mock
    Class1 class1;
    @Mock
    Class2 class2;
    @InjectMocks
    MainClass main;
}
```