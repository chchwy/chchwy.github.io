---
date: 2017-05-07
title: Windows 編譯 Ogre v2.1
isCJKLanguage: true
tags: [Ogre3D]
---
本文紀錄我在 Windows 10 上編譯 Ogre 2.1 的方法跟步驟。

![Ogre3D](/img/ogre3d-logo.svg)

Ogre 1.x 跟 2.x 的比較可以參考[這個連結](https://www.ogre3d.org/about/what-version-to-choose)。2.x 版現在已經改名叫 Ogre-Next 來跟原本的 Ogre3D 做出區隔，新版不管在效能還是視覺方面都有明顯提昇。

我的開發環境如下:

- 作業系統 Windows 10 Home
- 編譯器 Visual Studio 2019 Community
- 版本控制系統 git
- 建構系統 CMake 3.17

## 下載原始碼

目前 Ogre3D 原始碼放在 Github 上，一共需要下載兩份 source code:

1. Ogre3D 引擎本體 <https://github.com/OGRECave/ogre-next>
2. 第三方依賴函式庫 <https://github.com/OGRECave/ogre-next-deps>

把兩個 repo 放在同一個目錄下：

```
C:/OgreSDK/ogre
C:/OgreSDK/ogredeps
```

## CMake 建構系統

Ogre 用的建構系統是 [CMake][cmake]，一套跨平台的建構系統。這是我第一次用 CMake，花了一些時間才搞懂用法。

CMake 本身不能直接編譯程式。相對的，它要求使用者先寫一個全平台通用的 CMake 腳本 (通常叫 `CMakeLists.txt`)，CMake 再把這個腳本轉換成實際的專案。
比如 Windows 上跑 CMake 就產出 Visual Studio 專案。Mac 就是上產生 Xcode 專案。

[cmake]: https://cmake.org/  "CMake official site"

## 編譯依賴函式庫

`ogre-next-deps` 裡面包含了 Ogre3D 採用的第三方函式庫。

先打開 **CMake-GUI**。

![ogredeps](/img/cmake-ogredeps.png)

- **Where is the source code:** 填寫 Repo 的目錄位置
- **Where to build the binaries:** 按照慣例，源始碼目錄下加一層 build 子目錄

接著以下步驟

- 按 **Configure**，選擇編譯器版本 `Visual Studio 16 2019 Win64`
- 再按 **Generate** ，產生 VS 專案檔
- 最後按 **Open Project** 打開 Visual Studio solution
- 開啟 Visual Studio 之後，編譯 `ALL_BUILD` 專案，Debug/Release 都要
- 接著單獨編譯 `INSTALL` 專案，在 INSTALL 上按右鍵 Build，同樣 Debug/Release 都要
- 這樣子第三方函式庫的編譯就算完成了

## 編譯引擎

接著編譯 Ogre 引擎本體。記得要切換到 `v2-1` 的分支上

跟前一步一樣，打開 CMake GUI

- **Where is the source code:** 填 ogre 的源始碼目錄
- **Where to build the binaries:** 往下加一層子目錄 build

![](/img/cmake-ogre3d.png)

按下 Configure 按鈕之後呢，應該會跳出一些錯誤，別擔心，這是因為 CMake 無法定位依賴庫的位置，藉由設定 **OGRE_DEPENDENCIES_DIR**  這個欄位告訴 CMake 依賴庫的位置：

- **OGRE_DEPENDENCIES_DIR** 填入 `C:/OgreSDK/ogredeps/build/ogredeps` 或者你的路徑
- Windows 下可以選用 DirectX 或者 OpenGL3+ 來渲染，兩者都勾也行
- Paging、RTShaderSystem、Terrain 這三個模組目前跟 2.1 不相容，**不要勾**
- OGRE_BUILD_SAMPLES2 是 Ogre 2.1 版的官方範例集，勾起來
- 勾選 SSE2 硬體指令集，加速數學運算
- `OGRE_UNITY_BUILD` 可以大大縮短編譯的時間，建議勾選

![](/img/cmake-ogre3d-config.png)

- 再 Congifure 一次，應該就沒有錯誤訊息了
- 按下 Generate 產生 Ogre.sln
- Open Project 打開 Visual Studio
- 編譯 **ALL_BUILD** 專案，Debug/Release都要
- 編譯 **INSTALL** 專案，同樣 Debug/Release都要
- 到此為止全部完成，挑選任一 Sample 執行即可

下圖是 Ogre3D 的官方範例 Forwrad3D 的畫面：

![](/img/ogre3d-forward.jpg)

