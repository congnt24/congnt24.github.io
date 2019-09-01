---
layout: post
title: "Test: Integration Test Java app"
categories: test
image: "https://rf3nt2hsbjj3mg90j4dgw7x6-wpengine.netdna-ssl.com/aus/wp-content/uploads/sites/36/2014/09/Integration-Landing-page-banner.jpg"
tags: [backend, java, test, unit, integrate]
---
Integration Test is the phase in software testing in which individual software modules are combined and tested as a group. Integration testing is conducted to evaluate the compliance of a system or component with specified functional requirements

<!--more-->

## 1. Integration Test in Java using maven-failsafe-plugin
We use maven-failsafe-plugin to run integration test.
For example:
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-failsafe-plugin</artifactId>
    <version>2.22.2</version>
    <configuration>
        <skipTests>${skipIT}</skipTests>
        <includes>
            <include>**/*IT.java</include>
            <include>**/*Tests.java</include>
        </includes>
        <systemPropertyVariables>
            <env>test</env>
        </systemPropertyVariables>
    </configuration>
    <executions>
        <execution>
            <id>run-integration-tests</id>
            <phase>integration-test</phase>
            <goals>
                <goal>integration-test</goal>
                <goal>verify</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
## 2. Using library to do integration test
Usually, the framework you use also have some libs for you to do integration test. For example, when you create a Web Service in Vert.x or Spring, they also provive some libs for you to start your Web Service before running Test. This ensure your Service will be started before all the test run then stop when the test done.
- Vertx Unit: https://vertx.io/docs/vertx-unit/java/
- Spring: spring-boot-maven-plugin

## 3. Using Maven plugin to mock external service
We usually using maven plugin to start all dependencies service before running integration test. With this aproach, we can ensure that all the service already started before run test without write code in setUp, tearDown method in many test class.
Below are some maven plugin to help you integrate with external service
### 3.1 Relation database
Using H2
### 3.2 MongoDB
Using embedmongo
```xml
<plugin>
    <groupId>com.github.joelittlejohn.embedmongo</groupId>
    <artifactId>embedmongo-maven-plugin</artifactId>
    <version>0.4.1</version>
    <executions>
        <execution>
            <id>start</id>
            <goals>
                <goal>start</goal>
            </goals>
            <configuration>
                <version>3.6.3</version>
            </configuration>
        </execution>
        <execution>
            <id>mongo-import</id>
            <goals>
                <goal>mongo-import</goal>
            </goals>
        </execution>
        <execution>
            <id>stop</id>
            <goals>
                <goal>stop</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
### 3.3 Kafka
```xml
<plugin>
    <groupId>com.github.congnt24</groupId>
    <artifactId>kafka-maven-plugin</artifactId>
    <version>1.0.1</version>
    <configuration>
        <zookeeperPort>52181</zookeeperPort>
        <kafkaPort>59092</kafkaPort>
    </configuration>
    <executions>
        <execution>
            <id>pre-integration</id>
            <goals>
                <goal>start-kafka</goal>
            </goals>
        </execution>
        <execution>
            <id>post-integration</id>
            <goals>
                <goal>stop-kafka</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

---