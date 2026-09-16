+++
title = "Hermes（個人助理）"
description = "Matt 自用的 Hermes 個人助理：透過 Google 授權存取日曆等資料，僅供本人使用。"
template = "page.html"
+++

Hermes 是我（Matt Chang）在本機與訊息管道上使用的**個人 AI 助理**。

這個 Google OAuth 應用程式**只給我自己的 Google 帳號使用**，不是對外公開產品，也沒有第三方使用者註冊或付費服務。

## 它做什麼

在我明確指示時，Hermes 可以代我：

- 讀取與管理 **Google Calendar** 行程
- 在需要時存取 **Google Drive / Docs** 中與助理任務相關的檔案（依實際授權範圍）

典型用途：查今天行程、寫家庭／學校事件、整理待辦與簡報相關資料。

## 資料如何處理

- 授權後的存取權杖保存在我自己的電腦（Hermes 設定目錄），用於呼叫 Google API。
- **不會**把 Google 帳號資料賣給他人，也**不會**做成公開服務給其他人登入。
- 詳細說明見 [隱私權政策](/hermes-privacy/)。

## 聯絡

- 開發者：Matt Chang（chchwy）
- Email：chchwy@gmail.com
- 網站：https://chchwy.github.io
