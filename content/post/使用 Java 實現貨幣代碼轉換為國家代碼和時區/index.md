---
title: 使用 Java 實現貨幣代碼轉換為國家代碼和時區
date: 2024-01-25
categories:
    - Java
tags:
    - Java
---

```pom.xml
<!-- https://mvnrepository.com/artifact/com.ibm.icu/icu4j -->
<dependency>
    <groupId>com.ibm.icu</groupId>
    <artifactId>icu4j</artifactId>
    <version>74.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/com.neovisionaries/nv-i18n -->
<dependency>
    <groupId>com.neovisionaries</groupId>
    <artifactId>nv-i18n</artifactId>
    <version>1.29</version>
</dependency>
```

<hr />

```java
import com.ibm.icu.util.TimeZone;
import com.neovisionaries.i18n.CountryCode;
import com.neovisionaries.i18n.CurrencyCode;

import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // 設定貨幣代碼
        String code = "JPY";

        // 通過代碼獲取貨幣
        CurrencyCode currency = CurrencyCode.getByCode(code);

        // 獲取與該貨幣相關的國家列表
        List<CountryCode> countryCodeList = currency.getCountryList();

        System.out.format("%s (%s) is used by:\n", currency, currency.getName());

        for (CountryCode country : countryCodeList) {
            // 獲取國家的時區列表
            String[] timeZoneList = TimeZone.getAvailableIDs(country.name()); 
            // 印出國家及其時區資訊
            System.out.format("\t%s: %s \n\t%s\n",
                    country, country.getName(), Arrays.toString(timeZoneList));
        }
    }
}

// 結果：
// JPY (Yen) is used by:
//	JP: Japan 
//	[Asia/Tokyo, JST, Japan]
```