+++
title = '使用 Java 轉換 WebP 成其他圖片格式'
date = 2024-10-03T02:31:37+08:00
draft = false
+++

### **WebP**

WebP 是由 Google 開發的一種現代圖片格式，旨在提供比傳統圖片格式（如 JPEG、PNG）更高的壓縮效率。其主要特點包括：

1. **更高的壓縮效率**：WebP 可以在保留畫質的情況下壓縮圖片的文件大小，這意味著可以用更少的儲存空間傳輸圖片，尤其適合網頁優化。據 Google 表示，WebP 圖片通常比 JPEG 圖片小 25% 到 34%。

2. **支援無損和有損壓縮**：
  - 有損壓縮：與 JPEG 類似，會去掉圖片中的一些細節來減小文件大小，但 WebP 通常能更有效地壓縮圖片而不明顯降低畫質。
  - 無損壓縮：保留了原始圖片的所有數據，但文件大小也比 PNG 格式的無損壓縮圖像小。

3. **支援透明度（alpha 通道）**：WebP 支援透明背景，這是 PNG 格式的一大優勢，但 WebP 提供了比 PNG 更小的文件大小。

4. **動畫支援**：WebP 也支援動態圖片，類似於 GIF，但比 GIF 有更好的壓縮效率和畫質。

5. **跨平台支援**：大多數現代瀏覽器，如 Google Chrome、Firefox、Edge 和 Opera 都支援 WebP，且許多設計和開發工具也已經加入了對 WebP 的支持。


---


### **TwelveMonkeys**


TwelveMonkeys 是一個開源的 Java 圖像處理庫，為 Java 的 ImageIO 框架提供了額外的圖像格式支持。

下面這段程式碼的目的是讀取 Webp 網址，將它轉換成其他圖片格式
```java
import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.net.URL;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedImage webpImage = ImageIO.read(new URL("https://www.gstatic.com/webp/gallery/1.webp"));

        String[] formatNames = ImageIO.getWriterFormatNames();
        Set<String> formatSet = new HashSet<>();
        for (String format : formatNames) {
            formatSet.add(format.toLowerCase());
        }

        for (String format : formatSet) {
            try {
                ImageIO.write(webpImage, format, new File("1." + format));
                System.out.println("已保存 1." + format);
            } catch (IOException e) {
                System.out.println("無法保存格式：" + format);
                e.printStackTrace();
            }
        }
    }
}
```

未使用此庫之前，若讀取 Webp 檔案或網址時，會產生以下錯誤
```java
Exception in thread "main" java.lang.IllegalArgumentException: image == null!
	at java.desktop/javax.imageio.ImageTypeSpecifier.createFromRenderedImage(ImageTypeSpecifier.java:917)
	at java.desktop/javax.imageio.ImageIO.getWriter(ImageIO.java:1603)
	at java.desktop/javax.imageio.ImageIO.write(ImageIO.java:1539)
	at dev.chengtc.Main.main(Main.java:23)

```

使用方法相當簡單，只要引入依賴後，不需要修改任何既有的程式碼，就能擴充額外的格式，[官方網站](https://haraldk.github.io/TwelveMonkeys/)中有條列出哪些格式有支援讀或寫的功能。

```xml
<!-- https://mvnrepository.com/artifact/com.twelvemonkeys.imageio/imageio-core -->
<dependency>
    <groupId>com.twelvemonkeys.imageio</groupId>
    <artifactId>imageio-core</artifactId>
    <version>3.11.0</version>
</dependency>
```
最終運行結果
```java
已保存 1.jpg
已保存 1.tif
已保存 1.tiff
已保存 1.bmp
已保存 1.gif
已保存 1.png
已保存 1.wbmp
已保存 1.jpeg
```

