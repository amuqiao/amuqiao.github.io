---
title: Junit基本注解
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-04 09:57:17
img:
password:
summary: Before、After等使用方法
tags:
- java
- junit
categories:
- 后端

---

> unit中集中基本注解，是必须掌握的。

### 基础概念
- @BeforeClass – 表示在类中的任意public static void方法执行之前执行
- @AfterClass – 表示在类中的任意public static void方法执行之后执行
- @Before – 表示在任意使用@Test注解标注的public void方法执行之前执行
- @After – 表示在任意使用@Test注解标注的public void方法执行之后执行
- @Test – 使用该注解标注的public void方法会表示为一个测试方法

### 使用示例
输入
```java
package com.luffy.junit;

import org.junit.After;
import org.junit.AfterClass;
import org.junit.Before;
import org.junit.BeforeClass;
import org.junit.Test;

public class JunitDemo {
    // Run once, e.g. Database connection, connection pool
    @BeforeClass
    public static void runOnceBeforeClass() {
        System.out.println("@BeforeClass - runOnceBeforeClass");
    }

    // Run once, e.g close connection, cleanup
    @AfterClass
    public static void runOnceAfterClass() {
        System.out.println("@AfterClass - runOnceAfterClass");
    }

    // Should rename to @BeforeTestMethod
    // e.g. Creating an similar object and share for all @Test
    @Before
    public void runBeforeTestMethod() {
        System.out.println("@Before - runBeforeTestMethod");
    }

    // Should rename to @AfterTestMethod
    @After
    public void runAfterTestMethod() {
        System.out.println("@After - runAfterTestMethod");
    }

    @Test
    public void test_method_1() {
        System.out.println("@Test - test_method_1");
    }

    @Test
    public void test_method_2() {
        System.out.println("@Test - test_method_2");
    }
}
```
输出
```java
@BeforeClass - runOnceBeforeClass
@Before - runBeforeTestMethod
@Test - test_method_1
@After - runAfterTestMethod
@Before - runBeforeTestMethod
@Test - test_method_2
@After - runAfterTestMethod
@AfterClass - runOnceAfterClass
```

### 参考文档
- [认识Junit基本注解@Before、@After、@Test、@BeforeClass、@AfterClass](https://www.cnblogs.com/hezhiyao/p/9440277.html)