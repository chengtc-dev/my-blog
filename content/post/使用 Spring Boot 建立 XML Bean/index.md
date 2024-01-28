---
title: 使用 Spring Boot 建立 XML Bean
date: 2024-01-28
categories:
    - spring boot
tags:
    - spring boot
    - java
weight: 9
---

## Bean

是 Spring 框架中一個被管理的組件，它封裝了應用程式中的某個功能，並由 Spring 容器負責管理其生命週期和相關的配置。這種組件化的方式有助於提高程式碼的可維護性和擴展性。

## 範例

1. 建立 XML 配置文件： 在 src/main/resources 目錄下，創建一個新的 XML 配置文件，例如 applicationContext.xml。

2. 配置 XML 文件： 在 applicationContext.xml 中，定義你的 Bean。以下是一個簡單的例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myBean" class="com.example.MyBean">
        <!-- 假設 MyBean 有一個名為 message 的屬性 -->
        <property name="message" value="你好，Spring Boot！"/>
    </bean>

</beans>
```
在這個例子中，我們創建了一個 ID 為 myBean 的 Bean，並設置了一個屬性 message 的值。

3. 在 Spring Boot 應用程式中啟用 XML 配置： 在主應用程式類別（通常是包含 main 方法的類別）上添加 @ImportResource 注解，指定 XML 配置文件的位置。

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

@SpringBootApplication
@ImportResource("classpath:applicationContext.xml")
public class MySpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```
@ImportResource("classpath:applicationContext.xml") 將 applicationContext.xml 文件引入 Spring Boot 應用程式。

4. 撰寫 Bean 類別： 在指定的包（在這個例子中是 com.example）下創建 MyBean 類別，該類別包含 message 屬性的 setter 方法。

```java
public class MyBean {

    private String message;

    public void setMessage(String message) {
        this.message = message;
    }

    public String getMessage() {
        return message;
    }
}
```

如果可以的話，推薦使用 Java-based 配置和注解（Annotations）的方式，因為它提供更好的可讀性和可維護性。