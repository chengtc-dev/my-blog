---
title: 使用 Java 實現 HTML Minify
date: 2024-01-26
categories:
    - Java
tags:
    - Java
---

## HTML Minify

是指透過去除空格、換行和其他不必要的字符，以減小 HTML 文件大小的過程。這樣的優化有助於提高網頁加載速度和性能。

## 範例

1. 新增依賴可以通過 Maven 或 Gradle 進行，以下是 Maven 的例子 :

```pom.xml
<!-- https://mvnrepository.com/artifact/com.googlecode.htmlcompressor/htmlcompressor -->
<dependency>
    <groupId>com.googlecode.htmlcompressor</groupId>
    <artifactId>htmlcompressor</artifactId>
    <version>1.5.2</version>
</dependency> 
```

2. 執行後可以看到空格、換行和其他不必要的字符都被去除了 :

```java
public class Main {
    public static void main(String[] args) {
       HtmlCompressor htmlCompressor = new HtmlCompressor();
       System.out.println(htmlCompressor.compress(
                        "<!DOCTYPE html>\n" +
                        "<html lang=\"en\">\n" +
                        "<head>\n" +
                        "    <meta charset=\"UTF-8\">\n" +
                        "    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n" +
                        "    <title>My Website</title>\n" +
                        "</head>\n" +
                        "<body>\n" +
                        "\n" +
                        "    <header>\n" +
                        "        <h1>Welcome to My Website</h1>\n" +
                        "    </header>\n" +
                        "\n" +
                        "    <nav>\n" +
                        "        <ul>\n" +
                        "            <li><a href=\"#section1\">Section 1</a></li>\n" +
                        "            <li><a href=\"#section2\">Section 2</a></li>\n" +
                        "            <li><a href=\"#section3\">Section 3</a></li>\n" +
                        "        </ul>\n" +
                        "    </nav>\n" +
                        "\n" +
                        "    <section id=\"section1\">\n" +
                        "        <h2>Section 1</h2>\n" +
                        "        <p>This is the content of the first section.</p>\n" +
                        "    </section>\n" +
                        "\n" +
                        "    <section id=\"section2\">\n" +
                        "        <h2>Section 2</h2>\n" +
                        "        <p>This is the content of the second section.</p>\n" +
                        "    </section>\n" +
                        "\n" +
                        "    <section id=\"section3\">\n" +
                        "        <h2>Section 3</h2>\n" +
                        "        <p>This is the content of the third section.</p>\n" +
                        "    </section>\n" +
                        "\n" +
                        "    <footer>\n" +
                        "        <p>&copy; 2024 My Website. All rights reserved.</p>\n" +
                        "    </footer>\n" +
                        "\n" +
                        "</body>\n" +
                        "</html>\n"));
    }
}

// Here is the result
// <!DOCTYPE html> <html lang="en"> <head> <meta charset="UTF-8"> <meta name="viewport" content="width=device-width, initial-scale=1.0"> <title>My Website</title> </head> <body> <header> <h1>Welcome to My Website</h1> </header> <nav> <ul> <li><a href="#section1">Section 1</a></li> <li><a href="#section2">Section 2</a></li> <li><a href="#section3">Section 3</a></li> </ul> </nav> <section id="section1"> <h2>Section 1</h2> <p>This is the content of the first section.</p> </section> <section id="section2"> <h2>Section 2</h2> <p>This is the content of the second section.</p> </section> <section id="section3"> <h2>Section 3</h2> <p>This is the content of the third section.</p> </section> <footer> <p>&copy; 2024 My Website. All rights reserved.</p> </footer> </body> </html> 


```
