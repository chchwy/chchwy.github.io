## 我的應用程式為什麼那麼大？

你有注意過執行檔的檔案大小嗎？本文記錄我在縮小執行檔體積過程中的發現，以及使用 Visual Studio/Xcode 產生的 Link Map 來分析檔案內各模組的詳細佔比。

### 問題: 不能超過45MB

這年頭很少人在關注應用程式的體積了，硬碟便宜，網路快，一支應用程式的體積到底是 10MB 還是 100MB，一般人大概不太在意。

但是在特定情形下，還是要計較一下體積的。

我這次碰到的危機，就是工作上的產品硬體有嚴格的限制，內嵌軟體必須小於45MB，否則就放不進硬體裡。但是就這麼不巧，編譯出來的軟體有52MB這麼大，超過了45MB。

增加硬體規格是不可能的，因為大量產品已經在市場上流通。唯一辦法是縮小執行檔體積，塞進有限的硬體空間裡。

### 為什麼要分析檔案體積

團隊裡很多人提出了想法，像是拔掉很少人用的甲功能，移除實驗性的乙功能，或者刪除用不到的模組等等。但是不管哪個方法，我們都碰到一個困難：動手前，怎麼先得知可以減掉多少體積？

很可能，工程師花了三天時間拔掉甲功能，結果只縮減了50KB，對於整體大局根本沒有影響。

所以我們想要先有一份檔案體積分析報告，說明每個模組佔用的大小和百分比。否則我們根本沒辦法評估哪個作法有效，能不能達成。

我研究了之後，發現編譯器可以產生一個東西叫 Link Map，內含我們需要的資訊。

### 產生 Link Map 的方法

#### Visual Studio 

 /MAP 連結器選項

#### Xcode

![[xcode-link-map]]


### 參考連結
[/MAP (Generate Mapfile) | Microsoft Learn](https://learn.microsoft.com/en-us/cpp/build/reference/map-generate-mapfile?view=msvc-170)