+++
title = 'HTTP/1 to HTTP/2 to HTTP/3 摘要'
date = 2024-09-24T00:43:23+08:00
draft = false
+++

> Detailed Summary for [HTTP/1 to HTTP/2 to HTTP/3](https://www.youtube.com/watch?v=a-sBfyiXysI?autoplay=1) by [Monica](https://monica.im/)

### **HTTP 的演進：從 HTTP/1 到 HTTP/2 再到 HTTP/3**

- HTTP/1 在 1996 年推出，建立在 TCP（Transmission Control Protocol）之上，但每個請求都需要一個單獨的 TCP 連線。
- HTTP/1.1 在 1997 年推出，引入了「保持連線」（keep-alive）機制，允許重複使用同一個連線，減少了延遲。
- HTTP/1.1 還加入了 HTTP 流水線（HTTP pipelining），但因許多代理伺服器（proxy servers）無法正確處理，最終許多瀏覽器取消了這項支援。

---

### **頭部阻塞問題與解決方案**

- HTTP/1.1 的流水線存在「頭部阻塞」（Head-of-Line Blocking）問題，後續請求必須等待前一個請求完成。
- HTTP/2 引入了 HTTP 流（HTTP streams），每個流都是獨立的，不需按順序傳送或接收，解決了應用層的頭部阻塞問題。但傳輸層的問題仍然存在。
- HTTP/3 在 2020 年成為草案，在傳輸層使用了新的協議，進一步解決了頭部阻塞問題。

---

### **QUIC 協議的出現**

- QUIC 是一種新的傳輸協議，取代了 TCP，基於 UDP（User Datagram Protocol）。
- QUIC 引入了「流」（streams）作為傳輸層的主要元素。
- QUIC 的流可以共享同一個 QUIC 連線，無需額外的握手（handshake）來創建新的流。
- QUIC 的流是獨立傳輸的，一個流的封包（packet）丟失不會影響其他流。
- QUIC 使用「連線 ID」（Connection ID），實現了在不同 IP 位址和網路介面間快速可靠地移動連線。
- 即使 HTTP/3 剛被標準化，已有 25% 的網站使用它，並被許多瀏覽器支援。