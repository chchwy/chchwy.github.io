---
isCJKLanguage: true
date: 2021-05-17
title: "TortoiseGit 圖示消失"
tags: [Windows]
draft: true
---

幾天前，我的電腦又發生了小烏龜 TortoiseGit 圖示消失的問題，檔案總管裡看不見 git 狀態，很不方便。

關於發生的原因，簡單講，就是小烏龜用來顯示綠色勾勾的那個機制: Windows Icon Overlay，有限制最大數量 16 個。如果超過了 16 個怎麼辦? 名字排序，只顯示前 16 個。

問題點就是 Dropbox 佔了 10 個位置，而且非常無恥的在註冊表名字前面都插了三個空格，確保他家的圖示一定會在命名排序的最前面。所以如果電腦上同時裝了 Dropbox + TortoiseGit ，那小烏龜的圖示就有非常大的機會被擠到 16 個有效位置以外，無法正確顯示。

解法網路上已經很多，就是修改註冊表，調整順序，請參考[這個網頁][0]和[這個網頁][1]，都解釋得很詳細。

但是有時候 Dropbox 更新版本可能會把設定改回去，導致我不定時要手改註冊表，很麻煩，我就在想能不能自動化這個過程呢? 研究了一下，發現 Powershell 可以修改註冊表。

就寫了以下的 Powershell script 來解決問題:

<script src="https://gist.github.com/chchwy/5418022d47fa49481f71ba481f54c02a.js"></script>

這個腳本會把 Dropbox 原本的 Icon overlay 從十個砍到剩必要的四個，再把無恥的三空格改成一個空格。確保他們的順序在 TortoiseGit 後面。

PowerShell 操作註冊表的寫法，跟操作檔案非常相似。

- `Get-ChildItem`：列出註冊表子項目
- `Remove-Item`：刪除註冊表
- `Rename-Item`：重新命名
- `Split-Path -leaf $path`：從完整路徑快速取出檔名 

唯一要注意的點，註冊表路徑要用 `HKLM:\` 或 `HKCU:\` 來起頭。

所以比如說這個註冊表路徑
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
```
要寫成
```
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
```


[0]: https://dotblogs.com.tw/kevinya/2017/07/24/180237 "TorsoieGit 跟 Dropbox 相衝"
[1]: https://www.garethjmsaunders.co.uk/2015/03/22/managing-overlay-icons-for-dropbox-and-tortoisesvn-and-tortoisegit/
[2]: https://blog.netwrix.com/2018/09/11/how-to-get-edit-create-and-delete-registry-keys-with-powershell/
[3]: https://docs.microsoft.com/zh-tw/powershell/scripting/samples/working-with-registry-keys?view=powershell-7.1
