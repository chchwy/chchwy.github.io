+++
title = "Hermes 隱私權政策"
description = "Hermes 個人助理（Google OAuth）隱私權政策 — 僅供開發者本人使用。"
template = "page.html"
+++

**最後更新：2026-09-16**

本政策適用於 Google Cloud 專案上、名稱為 **Hermes**（或 Hermes 個人助理）的 OAuth 應用程式，由 **Matt Chang**（聯絡：chchwy@gmail.com）維護。

此應用程式是**個人自用工具**，不是面向大眾的商業服務。

## 1. 我們存取哪些 Google 資料

依 OAuth 同意畫面實際勾選的範圍，可能包括：

| 範圍 | 用途 |
|------|------|
| Google Calendar | 讀取、建立、修改、刪除行程與日曆清單，以便助理處理行程 |
| Google Drive | 讀寫與助理任務相關的雲端檔案（若已授權） |
| Google Docs | 讀寫與助理任務相關的文件（若已授權） |

應用程式**只**向 Google 請求完成上述個人助理功能所需的權限；實際範圍以你在 Google 同意畫面看到的為準。

## 2. 資料如何使用

Google 使用者資料僅用於：

- 在我的指示下查詢或更新日曆
- 在我的指示下讀寫相關 Drive／Docs 內容
- 維持登入狀態（OAuth access / refresh token 自動更新）

**不會**用於廣告、分析產品給第三方、訓練對外販售的模型服務，或提供給其他終端使用者。

## 3. 資料如何儲存

- OAuth **token**（含 refresh token）儲存在執行 Hermes 的裝置本機（例如 Hermes 設定目錄中的憑證檔）。
- 助理對話或任務過程中，行程／檔案內容可能短暫出現在本機記憶體、本機 session／log，或我自行設定的筆記工具中，以便完成當次任務。
- **不會**經營對外的多租戶伺服器來集中存放其他 Google 使用者的資料。

## 4. 資料分享

- **不會出售** Google 使用者資料。
- **不會**將 Google 使用者資料提供給廣告商或不相關的第三方。
- 僅在下列情況可能涉及第三方傳輸：呼叫 **Google API** 本身；或法律要求時。
- 若助理後端使用大型語言模型 API，我可能把**當次任務所需的摘要或片段**送出以產生回覆；這仍限於我自己發起的個人使用，且應避免不必要地傳送敏感內容。

## 5. 資料保留與刪除

- 可隨時到 [Google 帳號 → 第三方存取權](https://myaccount.google.com/permissions) 撤銷「Hermes」的存取；撤銷後應用程式無法再呼叫你的 Google API，本機 token 也會失效或應刪除。
- 本機憑證檔可手動刪除（Hermes 設定目錄中的 Google token／client 檔）。
- 撤銷授權後，Google 側的授權即終止；本機若仍留有舊檔，應一併刪除。

## 6. 安全

- 使用 Google 官方 OAuth 2.0；client secret／token 僅放在本機或我控制的環境，不當成公開程式碼提交。
- 裝置與磁碟安全由我自行負責（個人裝置）。

## 7. 兒童隱私

本應用不面向 13 歲（或當地法律規定年齡）以下兒童，亦無意收集其資料。

## 8. 政策變更

若使用方式或授權範圍有實質變更，會更新本頁之「最後更新」日期。繼續使用即表示知悉更新後的政策。

## 9. 聯絡方式

關於本應用或 Google 資料處理：

- 姓名：Matt Chang
- Email：chchwy@gmail.com
- 應用說明頁：https://chchwy.github.io/hermes/

本政策與[應用說明頁](/hermes/)所描述之功能一致。
