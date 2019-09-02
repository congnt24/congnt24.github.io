---
layout: post
title: "Test: Code coverage"
categories: test
image: "https://igorpopov.io/images/mutation-testing-100-percent.jpg"
tags: [java, maven, test]
---
In this post, we will setup jacoco maven plugin to run code coverage, checking the quality of your code.
<!--more-->
## 1. Setup code coverage with jacoco maven plugin

Example:
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.4</version>
    <configuration>
        <excludes>
            <exclude>com.github.congnt24/Main.class</exclude>
        </excludes>
    </configuration>
    <executions>
        <execution>
            <id>prepare-agent</id>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>default-report</id>
            <phase>verify</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
        <execution>
            <id>default-check</id>
            <phase>verify</phase>
            <goals>
                <goal>check</goal>
            </goals>
            <configuration>
                <rules>
                    <rule>
                        <element>BUNDLE</element>
                        <limits>
                            <limit>
                                <counter>COMPLEXITY</counter>
                                <value>COVEREDRATIO</value>
                                <minimum>0.60</minimum>
                            </limit>
                        </limits>
                    </rule>
                </rules>
            </configuration>
        </execution>
    </executions>
</plugin>
```
Run: mvn clean verify/test to run the code coverage
## 2. Explain
The above example we used jacoco-maven-plugin with 3 goal:
1. prepare-agent 

We will prepare jacoco before run code coverage
2. report in verify phrase

- jacoco:report will be hook in verify phrase, when run mvn command such as: **mvn test, mvn verify**. Maven's verify phrase will be include, so report goal will be execute.
- Jacoco will generate a report to the build target, so you can access the report in target/jacoco.exec or html report in target/site/jacoco/index.html


3. check in verify phrase

After jacoco:report executed, we will run jacoco:check to check you code is react the minimun code coverage or not. Here we defined the minimun coveredratio for Complexity is 60%. We can define other minimun value or change the counter to other value such as: 
- **COUNTER**: INSTRUCTION, LINE, BRANCH, COMPLEXITY, METHOD, CLASS
- **ELEMENT**: BUNDLE, PACKAGE, CLASS, SOURCEFILE or METHOD
- **COVEREDRATIO**: TOTALCOUNT, COVEREDCOUNT, MISSEDCOUNT, COVEREDRATIO, MISSEDRATIO

---

---