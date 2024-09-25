+++
title = 'Abstract Classes Compared to Interfaces'
date = 2024-09-26T01:00:16+08:00
draft = false
+++

> Detailed Summary for [Abstract Classes Compared to Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)

### **相似與不相似處**

抽象類別和介面都不能直接建立它們的實例，而且它們可以包含有實作或沒有實作的方法。

抽象類別可以宣告 non-static 和 non-final 的欄位，並定義 public、protected 和 private 的具體方法。
一個類別僅能繼承（extend）一個抽象類別。

介面所有欄位自動是 public、static 和 final，所有宣告或定義的方法都是 public。
一個類別能實作（implement）任意數量的介面。


---


### **使用時機**

以下幾種情境適用抽象類別：
1. 想在多個關係密切的類別中共享程式碼。
2. 預期子類別會有許多共同的方法或欄位，或需要使用非 public 的存取修飾子。
3. 需要宣告 non-static 和 non-final 的欄位。

以下幾種情境適用介面：
1. 預期被不相關的類別實作。
2. 想定義要執行的行為，但不關心誰來實作這些行為。
3. 想利用**能實作任意數量的介面**的特性。


---


### **範例**

AbstractMap 是 JDK 中的抽象類別，屬於集合框架（Collections Framework）。它的子類別（如 HashMap、TreeMap、ConcurrentHashMap）共享許多由 AbstractMap 定義的方法，如 get、put、isEmpty、containsKey 和 containsValue。

HashMap 類別實作了 Serializable、Cloneable 和 Map<K, V> 介面。透過這些介面，我們可以知道 HashMap 的實例可以被複製，是可序列化的（serializable，意味著可以轉換成位元組流），並具備映射（map）的功能。此外，Map<K, V> 介面增加了許多預設方法，如 merge 和 forEach，已實作此介面的舊類別不需要重新定義這些方法。
