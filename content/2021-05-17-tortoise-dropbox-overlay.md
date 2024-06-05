---
date: 2021-05-17
title: "寫 Powershell 解決 TortoiseGit 圖示消失問題"
taxonomies:
  tags: [PowerShell, Troubleshooting ]
---

幾天前，小烏龜 TortoiseGit 圖示消失的問題又發生了，我在檔案總管上看不見 git 狀態，很不方便。

![icon-overlay](/img/icon-overlay.png) 

上圖是正常狀態，有綠色小勾勾顯示 git 狀態。

發生的原因，就是顯示綠色勾勾的那個機制: Windows Icon Overlay 有數量限制 16 個。這 16 個位置是所有應用程式共享的，包括 Dropbox、TortoiseGit、OneDrive 等等。

如果超過了 16 個怎麼辦？名字排序，顯示前 16 個。

我去打開 Windows Icon Overlay 的清單，發現 Dropbox 一隻程式就佔用了**10 個位置**，而且**非常無恥**的在名字前面插三個空格，確保他家的圖示一定會佔住最前面的位置。所以一旦電腦上同時裝了 Dropbox + TortoiseGit，小烏龜的圖示就有很大的機會被擠到 16 個有效位置以外，無法正確顯示。

網路上已經很多人貼解法了，改註冊表，調整順序，都很詳細，我就不贅述了：

- [Dropbox與小烏龜Tortorise的圖示icon相衝問題解決][0]
- [Managing overlay icons for Dropbox and TortoiseSVN and TortoiseGit][1]

但是註冊表改好後，這個問題仍然會不定時出現。因為 Dropbox 每次更新版本後會把註冊表改回去！

所以我就在想能不能把解法自動化，寫成程式? 

研究了一下，發現 Powershell 可以修改註冊表。就花了一點時間寫 Powershell:

我發現 PowerShell 操作註冊表，跟操作檔案非常相似。

- `Get-ChildItem`：列出註冊表子項目
- `Remove-Item`：刪除註冊表
- `Rename-Item`：重新命名
- `Split-Path -leaf $path`：從完整路徑快速取出檔名

註冊表路徑以 `HKLM:\` 或 `HKCU:\` 起頭。`HKLM:\` 表示 Local Machine， `HKCU:\` 表示 Current User。

比如這個註冊表路徑
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
```
在 powershell 裡面就要寫成這樣：
```
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
```

以下是我的 PowerShell 腳本。砍掉比較少見的 Dropbox 圖示，留下必要的四個，再把無恥的三空格改成一個空格。確保他們的順序在 TortoiseGit 後面。
<script src="https://gist.github.com/chchwy/5418022d47fa49481f71ba481f54c02a.js"></script>

## 參考網頁

- [Dropbox與小烏龜Tortorise的圖示icon相衝問題解決][0]
- [Managing overlay icons for Dropbox and TortoiseSVN and TortoiseGit][1]
- [How to Get, Edit, Create and Delete Registry Keys with PowerShell][2]
- [PowerShell: 使用登錄機碼][3]

[0]: https://dotblogs.com.tw/kevinya/2017/07/24/180237 "TorsoieGit 跟 Dropbox 相衝"
[1]: https://www.garethjmsaunders.co.uk/2015/03/22/managing-overlay-icons-for-dropbox-and-tortoisesvn-and-tortoisegit/
[2]: https://blog.netwrix.com/2018/09/11/how-to-get-edit-create-and-delete-registry-keys-with-powershell/
[3]: https://docs.microsoft.com/zh-tw/powershell/scripting/samples/working-with-registry-keys?view=powershell-7.1
