+++
title = 'Builder Pattern 介紹'
date = 2024-09-11T00:30:35+08:00
draft = false
+++

在工作上常常會需要串接第三方 API ，不免俗的就會需要建立許多 Request 及 Response 物件，分別用來發送與接受資料，要是運氣夠好，第三方需要的參數夠少，可以直接在 Request 物件中使用 getter 與 setter 方法設定參數。

```java
public class Request {
    private String url;                  // 必填
    private String method;               // 必填
    private String body;                 // 選填
    private Map<String, String> headers; // 選填
    private Map<String, String> params;  // 選填

    // getter 與 setter 省略
}
```

上面這種方法稱為 Java Bean，它的優點是非常好理解，搭配上 IDE 的自動產生功能，能快速產出 getter 與 setter 方法，但是缺點是在物件建立後，物件仍然能夠被 setter 方法修改，也不能保證必填屬性一定有值，關於不能保證必填屬性一定有值這點，可能有人會想使用多個多載的建構子來處理不同數量的參數。

```java
public class Request {
    private String url;                  // 必填
    private String method;               // 必填
    private String body;                 // 選填
    private Map<String, String> headers; // 選填
    private Map<String, String> params;  // 選填

    // 第一個建構子，僅包含必填參數
    public Request(String url, String method) {
        this.url = url;
        this.method = method;
    }

    // 第二個建構子，包含必填參數和 body
    public Request(String url, String method, String body) {
        this(url, method); // 調用包含必填參數的建構子
        this.body = body;
    }

    // 第三個建構子，包含必填參數、body 和 headers
    public Request(String url, String method, String body, Map<String, String> headers) {
        this(url, method, body); // 調用包含 body 的建構子
        this.headers = headers;
    }

    // 第四個建構子，包含必填參數、body、headers 和 params
    public Request(String url, String method, String body, Map<String, String> headers, Map<String, String> params) {
        this(url, method, body, headers); // 調用包含 headers 的建構子
        this.params = params;
    }

    // getter 與 setter 略
```

上面這種方法稱為可伸縮建構子（telescoping constructor），當屬性較少時，是不錯的方法，但是一旦屬性增加時，容易變得難以維護，也無法直觀地知道建構子的哪個位置要填什麼值，為了解決上述問題，就有了建造者模式（Builder Pattern）供我們優雅地建立物件。

```java
public class Request {
    private final String url;                  // 必填
    private final String method;               // 必填
    private final String body;                 // 選填
    private final Map<String, String> headers; // 選填
    private final Map<String, String> params;  // 選填

    // 私有建構子，只有 Builder 可以訪問
    private Request(Builder builder) {
        this.url = builder.url;
        this.method = builder.method;
        this.headers = builder.headers;
        this.params = builder.params;
        this.body = builder.body;
    }

    // 建立 Builder 類別
    public static class Builder {
        private final String url;                              // 必填
        private final String method;                           // 必填
        private String body;                                   // 選填
        private Map<String, String> headers = new HashMap<>(); // 預設為空
        private Map<String, String> params = new HashMap<>();  // 預設為空

        // 必填項目透過建構子設定
        public Builder(String url, String method) {
            this.url = url;
            this.method = method;
        }

        // 設定 headers
        public Builder headers(Map<String, String> headers) {
            this.headers = headers;
            return this;
        }

        // 設定 params
        public Builder params(Map<String, String> params) {
            this.params = params;
            return this;
        }

        // 設定 body
        public Builder body(String body) {
            this.body = body;
            return this;
        }

        // 建構最終的 Request 物件
        public Request build() {
            return new Request(this);
        }
    }
}
```

上面這種方法稱為建造者模式（Builder Pattern），它將建構細節封裝在 Builder 類別中，透過 Builder 我們可以按需設置 Request 類別的各種屬性，最後通過 build() 方法創建物件。

```java
public class Main {
    public static void main(String[] args) {
        // 準備 headers 和 params
        Map<String, String> headers = new HashMap<>();
        headers.put("Content-Type", "application/json");

        Map<String, String> params = new HashMap<>();
        params.put("book", "java");
        params.put("page", "1");

        // 使用 Builder 構建 Request 物件
        Request request = new Request.Builder("https://api.example.com/search", "GET")
                .headers(headers)
                .params(params)
                .build();
    }
}
```

## 總結

Builder Pattern 是一個非常實用的設計模式，特別是在處理構建複雜物件時。它不需要過多的建構子、更容易且直觀地添加可選屬性、保證必填屬性一定有值並且在物件建立後，物件內的屬性無法被修改。