---
date: 2019-08-14
title: "Chocolatey 套件管理，重灌 Windows 後的好幫手"
taxonomies:
  tags: [開發日常, 生產力]
---

上個月，我又重灌了 Windows。

重灌後，我給自己設了一個挑戰：不手動下載任何安裝檔，就把電腦恢復成可工作狀態。

我想要這樣挑戰，是因為每次重灌電腦後，就是惡夢般的漫長的軟體安裝之旅。去每個軟體的官方網站，下載，等待，安裝，有時候要重新開機，無限迴圈。過程比安裝 Windows 本身還要漫長。

事後來看，雖然「無手動下載」的目標還沒辦法達成百分之百。但是除了少數例外，我已經可以自動化安裝八成以上的常用軟體了。主要靠兩個的軟體套件管理：Chocolatey 和 Microsoft Store。

## Chocolatey 教學

[Chocolatey][choco] 是命令列套件管理工具。類似 Ubuntu 的 `apt-get` 讓你一行指令完成軟體的安裝，同樣的概念在套用在 Windows 上就是 Chocolatey。許多常見的 Windows 軟體都已經在 Chocolatey 上架。

[choco]: https://chocolatey.org/ "Chocolatey Official Website"

### 安裝 Chocolatey

打開具有管理員權限的 Powershell，複製貼上官網的安裝指令 :<br/>
<https://chocolatey.org/install#install-with-powershellexe>

### 常用指令

Chocolatey 的指令名稱是 `choco`，大多數操作需要管理員權限。

安裝套件 (以 7-zip 為例)
```
choco install 7zip
```

反安裝套件
```
choco uninstall 7zip
```

列出已安裝的套件
```
choco list --local
```

檢查套件是否有新版本
```
choco outdated
```

更新所有套件到最新版
```
choco upgrade all
```

這是我的 Chocolatey 安裝清單：
```bash
choco install -y 7zip                # 萬用壓縮軟體
choco install -y geekuninstaller     # 強大的反安裝工具
choco install -y firefox             # 我的慣用瀏覽器
choco install -y googlechrome        # 有時候需要另一個瀏覽器
choco install -y everything          # 最強大的檔案搜尋工具
choco install -y dropbox             # Dropbox 客戶端
choco install -y golang              # Go lang
choco install -y cmake               # CMake
choco install -y vscode              # VS Code
choco install -y notepadplusplus     # 老牌文字編輯器 Notepad++
choco install -y tortoisegit         # 小烏龜 Git
choco install -y git-fork            # 另一款輕巧的 git 圖形化工具
choco install -y beyondcompare       # 超級好用的 diff/merge 工具
choco install -y filezilla           # FTP 客戶端
choco install -y foxitreader         # 我慣用的 PDF viewer
choco install -y hugo                # 靜態博客產生器
choco install -y pandoc              # 強大的文件轉換工具
choco install -y paint.net           # 免費的小畫家強化版
choco install -y workflowy           # 條列式筆記工具
choco install -y libreoffice-fresh   # 沒辦法，有時候還是要處理.docx
choco install -y mp3tag              # MP3 標籤編輯
choco install -y treesizefree        # 追蹤硬碟空間使用
choco install -y ccleaner            # 清理垃圾檔案
choco install -y teamviewer          # Teamviewer 遠端桌面
choco install -y 4k-video-downloader # 影片/音樂抓取
choco install -y xnviewmp            # 看圖軟體
choco install -y foobar2000          # 聽歌軟體
choco install -y steam               # 娛樂軟體
choco install -y vlc                 # 開源影片播放軟體
choco install -y k-litecodecpack-standard # k-lite 萬用影音播放 codec
```

只要一個 Powershell 腳本就可以把清單上的軟體全裝好了，之後升級版本也只要一個命令，異常方便。

### Microsoft Store

另外有幾款軟體，雖然也可以用 Chocolatey，但是我發現 Microsoft Store 上的版本更穩定。像是 Evernote 和 Line 的桌面版。用 Microsoft Store 安裝軟體的體驗也很好，一鍵安裝，無縫升級。

### 其他沒辦法自動安裝的軟體

因為某些原因，以下軟體我還是必須手動安裝：

- Visual Studio (沒有其他安裝方法)
- Qt (沒有其他安裝方法)
- git (安裝過程中我需要修改選項)
- Slack (我刻意安裝 32-bit 的版本，希望能限制記憶體用量，但是還沒驗證過有沒有效XD)
