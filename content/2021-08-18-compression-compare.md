---
date: 2021-08-18

title: "隨手比較壓縮演算法：7zip/GZIP/LZ4 "
taxonomies:
  tags: [開發日常]
---

最近有定期從遠端 Linux 主機上拉檔案下來的需求，內容是遊戲開發的資源檔，每次數百GB。因為檔案實在太大，不得不找一些壓縮檔案的方法，希望縮短下載時間。

我隨手估狗了五種壓縮方法如下：

1. 7zip 最快速壓縮 (壓縮演算法是LZMA2)
2. 7zip 一般壓縮 (同上，只有參數不同)
3. Gzip
4. LZ4
5. Zip

實際跑的指令如下：
```bash
time 7z -mx1 a test.7z ./trunk/             # 7zip 最快速壓縮
time 7z -mx3 a test.zip ./trunk/            # 7zip 一般壓縮      
time tar -czvf test.tar.gz ./trunk/         # GZIP
time tar -I lz4 -cvf test.tar.lz4 ./trunk/  # LZ4
time zip -r -s 10G test.zip ./trunk/        # ZIP
```

挑選的標準很簡單，不要太冷門，壓縮/解壓縮工具在 Windows/Linux 上可以方便操作。如果有人知道什麼特別優秀的壓縮演算法麻煩告訴我。

## 測試環境

測試樣本總共 129GB，內容物大約為 45% FBX模型、45% TGA貼圖、10% 其他有的沒的。129GB 只是全部檔案的一部份，但我認為已經足夠有代表性來推斷出整體時間。

測試的機子是 Vultr VPS 640G 方案，作業系統 Ubuntu 21.04。

![Vultr](/img/vultr-640.png)

## 測試結果

表格內是五種方法的測試結果，包括壓縮後的檔案大小，壓縮比，以及壓縮時間

<style>
table { border:solid 0px #cccccc; }
th, td {border:1px solid #aaa; padding: 5px;}
</style>

方法           | 壓縮時間 | 壓縮前大小 | 壓縮後大小 | 壓縮比
---------------|---------|-----------|-----------|------
7zip 最快速壓縮 |  22m55s | 129 GB    | 38 GB     |  29.5%
7Zip 一般壓縮   |   43m3s | 129 GB    | 36.8 GB   |  28.5%
Gzip           |  75m33s | 129 GB    | 57.2 GB   |  33.1%
LZ4            |   8m21s | 129 GB    | 42.7 GB   |  44.3%
Zip            |  79m28s | 129 GB    | 43 GB     |  33.3% 

從表格來看，7zip (也就是LZMA2演算法) 的壓縮率全場最佳，GZIP/ZIP 次之，LZ4 的壓縮率最差。壓縮速度則是 LZ4 最快，7zip 次之，Zip/Gzip 最慢。

接著根據我家的平均網速 4.5MB/sec，計算出下載檔案需要的時間，再加總起來

方法           | 壓縮時間 (秒) | 下載時間 (秒) | 總時間 
---------------|--------------|--------------|-----
7zip 最快速壓縮 |  1375        |  8647        |  167分鐘
7zip 一般壓縮   |  2583        |  8374        |  182分鐘
Gzip           |  4533        |  9716        |  237分鐘
LZ4            |   501        | 13016        |  225分鐘
Zip            |  4768        |  9784        |  242分鐘

結果出來了，從總時間來看，**7zip 最快速壓縮**是最佳方案，只要167分鐘就可以把整包129GB檔案下載到我的本機。

## 方案比較

CPU 使用量，依我的粗略觀察，LZ4 CPU 使用量最低，Gzip/Zip 單核使用量 100%，7zip 則是八核全部用滿，八核心全部 100%。所以雖然 7zip 總時間最短，但是代價是吃光全部主機資源。

7zip 從最快速壓縮 `-mx1` 換成一般壓縮 `-mx3` 只增加了 1% 壓縮率，但是壓縮時間長了快一倍，不太值得。

LZ4 演算法標榜超快速壓縮，名符其實，只用了八分多鐘壓完全部檔案，海放其他演算法。但是因為檔案總體積太大，所以壓縮率普通的缺點就被突顯出來，如果下次壓比較小的檔案，我可能會考慮LZ4。

Zip/Gzip 骨子裡是同一套壓縮演算法，所以不意外表現非常接近。壓縮比不差，但是真的壓太久了。

另外 7zip 和 Zip 壓縮時可以指定輸出成多個固定尺寸的檔案，減少一次性傳輸超大檔案的失敗機會。
