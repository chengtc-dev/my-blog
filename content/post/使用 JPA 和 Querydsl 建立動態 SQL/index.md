---
title: 使用 JPA 和 Querydsl 建立動態 SQL
date: 2024-01-26 16:10:00+0800
categories:
    - jpa
tags:
    - jpa
    - java
weight: 4
---

## JPA
JPA 是 Java Persistence API 的縮寫，是一種 Java EE 的標準規範，用於透過ORM（物件關聯映射）方式來管理 Java 應用程式中的資料庫。JPA 提供了對關聯資料庫的標準化存取，並且能夠將 Java 物件映射到資料表。

## Querydsl
Querydsl 是一種用於構建 type-safe 的動態 SQL 查詢框架。它提供了一種特殊的 DSL（Domain Specific Language），允許以 type-safe 的方式構建動態查詢，而不是依賴於字串。

## 範例
1. 新增依賴（5.0.0版本之後，無需配置 Annotation Processing Tool）可以通過 Maven 或 Gradle 進行，以下是 Maven 的例子 :

```pom.xml
<!-- Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- Querydsl JPA -->
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
    <classifier>jakarta</classifier>
    <version>5.0.0</version> <!-- Use the latest version available -->
</dependency>

<!-- Querydsl APT -->
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
    <classifier>jakarta</classifier>
    <version>5.0.0</version> <!-- Use the latest version available -->
    <scope>provided</scope>
</dependency>
```


2. 要使用 Querydsl，需要繼承 QuerydslPredicateExecutor 介面，如下所示 :

```java
interface UserRepository extends CrudRepository<User, Long>, QuerydslPredicateExecutor<User> {
}
```


3. 接著使用 Querydsl 的 Predicate 實例建立 type-safe 查詢

```java
Predicate predicate = user.firstname.equalsIgnoreCase("dave")
	.and(user.lastname.startsWithIgnoreCase("mathews"));

userRepository.findAll(predicate);
```

>  [https://docs.spring.io/spring-data/jpa/reference/repositories/core-extensions.html](https://docs.spring.io/spring-data/jpa/reference/repositories/core-extensions.html)
