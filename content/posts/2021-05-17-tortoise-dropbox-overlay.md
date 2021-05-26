---
isCJKLanguage: true
date: 2021-05-17
title: "TortoiseGit 圖示消失"
tags: [Windows]
draft: false
---

幾天前，小烏龜 TortoiseGit 圖示消失的問題又發生了，我在檔案總管上看不見 git 狀態，很不方便。

![icon-overlay](/img/icon-overlay.png)

問題的原因，簡單講，就是顯示綠色勾勾的那個機制: Windows Icon Overlay，有數量限制 16 個。這 16 個位置是所有應用程式包括 Dropbox、TortoiseGit、OneDrive 等等共享的。

如果超過 16 個呢？就用名字排序，顯示前 16 個。

問題是 Dropbox 它一支程式就佔了 10 個位置，而且在註冊表名字前面插了三個空格，確保他家的圖示一定在排序的最前面，非常無恥。

所以只要電腦上同時裝 Dropbox + TortoiseGit ，那小烏龜的圖示有很大的機會被擠到 16 個有效位置以外，無法正確顯示。

解法網路上已經很多，修改註冊表，調整順序，參考[這個網頁][0]和[這個網頁][1]，都解釋得很詳細。

但是註冊表改好後，這個問題仍然可能不定時出現。比如 Dropbox 更新版本時會把註冊表改回去，很麻煩。

我就在想能不能自動化這個過程呢? 研究了一下，發現 Powershell 可以修改註冊表。
花了一點時間，把解法寫成了以下的 Powershell script:

<script src="https://gist.github.com/chchwy/5418022d47fa49481f71ba481f54c02a.js"></script>

這個腳本會砍掉少用的 Dropbox 圖示，只留下必要的四個，再把無恥的三空格改成一個空格。確保他們的順序在 TortoiseGit 後面。

經過這支腳本，我學到的怎麼用 PowerShell 操作註冊表，跟操作檔案非常相似。

- `Get-ChildItem`：列出註冊表子項目
- `Remove-Item`：刪除註冊表
- `Rename-Item`：重新命名
- `Split-Path -leaf $path`：從完整路徑快速取出檔名 

特別要注意的點是磁碟機，註冊表路徑要用 `HKLM:\` 或 `HKCU:\` 來起頭。

所以比如說這個註冊表路徑
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
```
要寫成
```
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
```

## 參考網頁

- [Dropbox與小烏龜Tortorise的圖示icon相衝問題解決][0]
- [Managing overlay icons for Dropbox and TortoiseSVN and TortoiseGit][1]
- [How to Get, Edit, Create and Delete Registry Keys with PowerShell][2]
- [使用登錄機碼 Microsoft][3]

[0]: https://dotblogs.com.tw/kevinya/2017/07/24/180237 "TorsoieGit 跟 Dropbox 相衝"
[1]: https://www.garethjmsaunders.co.uk/2015/03/22/managing-overlay-icons-for-dropbox-and-tortoisesvn-and-tortoisegit/
[2]: https://blog.netwrix.com/2018/09/11/how-to-get-edit-create-and-delete-registry-keys-with-powershell/
[3]: https://docs.microsoft.com/zh-tw/powershell/scripting/samples/working-with-registry-keys?view=powershell-7.1
