+++
title = 'Everything You Need to Know About DNS: Crash Course System Design #4 摘要'
date = 2024-09-25T00:29:19+08:00
draft = false
+++

> Detailed Summary for [Everything You Need to Know About DNS: Crash Course System Design #4](https://www.youtube.com/watch?v=27r4Bzuj5NQ?autoplay=1)

### **什麼是DNS？**

DNS（網域名稱系統，Domain Name System）是網際網路的重要基礎。它將我們容易記住的網域名稱（例如 google.com）轉換成電腦使用的 IP 位址。DNS 伺服器有一個階層結構，每一層都有不同的功能。

---

### **DNS查詢流程**

當你在瀏覽器中輸入一個網址時，瀏覽器會向 DNS 解析器（DNS Resolver，例如你的網路服務提供商或像 Cloudflare 這樣的公司）詢問對應的 IP 位址。如果解析器不知道答案，它會去尋找擁有該資訊的權威名稱伺服器（Authoritative Nameserver）。

---

### **DNS階層**

DNS 伺服器主要有三個層級：

1. 根名稱伺服器（Root Nameservers）：知道所有頂級網域（TLD）伺服器的 IP 位址。
2. 頂級網域（TLD）名稱伺服器：管理特定的頂級網域，如 .com 或 .org。
3. 權威名稱伺服器（Authoritative Nameservers）：儲存特定網域的 IP 位址資訊。

---

### **DNS查詢步驟解析**

DNS 查詢的步驟如下：

1. 瀏覽器快取：先檢查是否已經有對應的 IP 位址。
2. 作業系統快取：如果沒有，檢查電腦的系統快取。
3. DNS 解析器：聯絡你的 DNS 解析器詢問。
4. 根伺服器：解析器詢問根名稱伺服器。
5. TLD 伺服器：根伺服器指引到正確的 TLD 伺服器。
6. 權威名稱伺服器：TLD 伺服器再指向權威名稱伺服器。
7. 取得 IP 位址：最後，從權威名稱伺服器獲得正確的 IP 位址。

---

### **更新DNS記錄**

在高流量的系統中更新 DNS 記錄時，因為 TTL（存活時間，Time to Live）的設定，DNS 更新可能會很慢。為了加速更新：

- 提前降低 TTL：在更改前將 TTL 設定得較低，讓更新更快傳播。
- 保持舊伺服器運行：在新記錄生效前，繼續運行舊的伺服器，確保服務不中斷，直到流量減少。
