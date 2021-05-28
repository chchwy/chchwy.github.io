---
date: "2015-06-14"
isCJKLanguage: true
title: "「因為這個系統上已停用指令碼執行，所以無法載入」"
tags: [PowerShell]
---

每個學 PowerShell 的人必然碰到的錯誤。

Windows 因為安全性考量，PowerShell 腳本(副檔名.ps1)預設是禁止執行的。所以同樣的命令，命令視窗直接打可以跑，但是存成檔案就會出現這個讓人摸不著頭腦的錯誤。

手動打開權限的作法如下：

用「系統管理員」身份打開 PowerShell，再輸入以下命令：

```code
Set-ExecutionPolicy RemoteSigned
```

就可以解鎖 PowerShell Script 了。

`Execution Policy` 是 PowerShell 的安全機制。`Remote Signed` 的意思是從網路上抓下來的 .ps1 要檢查數位簽章，但是本地的 PowerShell 檔案直接放行。

更多 Execution Policy 等級可以參考[這裡][0]。

[0]: http://gelis-dotnet.blogspot.tw/2010/10/win72008-server-powershell.html
