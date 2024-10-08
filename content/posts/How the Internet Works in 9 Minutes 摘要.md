+++
title = 'How the Internet Works in 9 Minutes 摘要'
date = 2024-09-23T01:28:35+08:00
draft = false
+++

> Detailed Summary for [How the Internet Works in 9 Minutes](https://www.youtube.com/embed/sMHzfigUxz4?autoplay=1)

### **網際網路的關鍵組成部分**

- **網路邊緣 (Edge of the Network)**：
    - 網路邊緣包括終端設備，例如桌上型電腦、伺服器、手機和物聯網裝置。
    - 終端設備分為客戶端和伺服器端。客戶端通常是個人裝置，伺服器端則是功能較強大的設備。
- **存取網路 (Access Network)**：
    - 存取網路將終端設備連接到路由器，主要分為三種：
        1. **家庭存取網路 (Home Access Network)**：家用網路，透過光纖、DSL或Wi-Fi等方式，將家中的設備（如電腦、手機）連接到網際網路。
        2. **機構存取網路 (Enterprise Access Network)**：公司、學校或政府機構使用的網路，通常透過有線以太網或Wi-Fi來連接多個設備到網際網路。
        3. **行動存取網路 (Mobile Access Network)**：使用4G、5G等行動通訊技術，讓手機、平板等行動裝置透過電信業者的基站連接到網際網路。

---

### **網路核心 (Network Core)**

- **封包路由與鏈路 (Packet Routers and Links)**：
    - 網路核心由封包路由器和鏈路構成，負責將資料封包從一個網路傳送到另一個。
    - 網路核心使用「封包交換 (Packet Switching)」的方式，將資料拆成小封包，能同時處理多個通訊，並提供靈活的路由選擇。
- **封包轉發與路由 (Packet Forwarding and Routing)**：
    - 封包轉發：將封包從路由器的輸入端移至適當的輸出端。
    - 路由：決定封包從來源到目的地的完整路徑。

---

### **BGP路由協定 (BGP Routing Protocol)**

- **BGP（邊界網關協定，Border Gateway Protocol）**：
    - BGP是網際網路的重要路由協定，允許不同的自治系統（AS）宣告可路由的IP區段，並讓路由器選擇最佳路徑。
    - 路由過程是動態且適應性的，當連線中斷或擁塞時，路由演算法能快速重新計算路徑。
- **網路協定 (Network Protocols)**：
    - 網際網路上的所有通訊都依賴協定進行，協定是網路的語言和規則。
    - 常見的協定有TCP、UDP、IP和HTTP，各自負責不同的網路功能。

---

### **TCP/IP模型 (TCP/IP Model)**

- **四層架構 (Four-Layer Architecture)**：
    - TCP/IP模型包含四個層級：
        1. **應用層 (Application Layer)**：負責應用程式與網路的互動，像是HTTP、SMTP和FTP。
        2. **傳輸層 (Transport Layer)**：確保應用程式間的資料傳輸穩定，使用TCP或UDP協定。
        3. **網路層 (Network Layer)**：負責資料封包的路由和定址，使用IPv4或IPv6協定。
        4. **資料連結層 (Data Link Layer)**：管理設備之間的物理連接，負責硬體層面的通訊。
