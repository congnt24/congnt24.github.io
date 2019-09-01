---
layout: post
title: "Test: Unit Test Java app with junit, mockito"
categories: test
image: "https://miro.medium.com/max/1400/1*X4lvnNBC7T3qjuWx007llw.png"
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
### 4.1 Using mockito
We will use @ExtendWith(MockitoExtension.class) and @MockitoSettings(strictness = Strictness.LENIENT) annotations for your class test.
Then using @Mock, @InjectMocks to mock object link below example:
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
    
    @Test
    public void method1_should_return_1(){
        Mockito.when(class1.method1(any())).thenReturn(1);
        Mockito.doNothing().when(class2).method2(anyString());
        main.doSomethingMethod();
    }
}
```
### 4.2 Declaring mock
- To mock a method that have return value, use: when().thenReturn()
- We can custom mock action by using thenAnswer(), you can do something in there and return a value.
- To mock a void method we can use. doNothing().when(class).method(any());
### 4.3 Verify mock
- Verify the mock method is running how many time, and the params value we use: verify()

For example:
```java
 Mockito.verify(service, Mockito.times(1)).save(Mockito.eq("123"), any());
 Mockito.verify(service, Mockito.times(2)).query(123, 12);
```

---