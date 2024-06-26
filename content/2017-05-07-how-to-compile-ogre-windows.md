---
date: 2017-05-07
title: 編譯 Ogre3D Next 引擎
taxonomies:
  tags: [Ogre3D]
---

本文紀錄我編譯 Ogre Next 引擎的方法跟步驟。

Ogre3D 2.x 分支已經改名叫 Ogre-Next，跟原本的 Ogre3D 1.x 版本引擎做出區隔。這篇文章裡提到的 Ogre 都是指 Ogre-Next。[新舊版引擎的比較][0]。

[0]: (https://www.ogre3d.org/about/what-version-to-choose) "Ogre What version to choose"

## 取得原始碼

目前 Ogre3D 原始碼放在 Github 上，需要下載兩份 source code:

1. Ogre3D 引擎本體 <https://github.com/OGRECave/ogre-next>
2. 第三方依賴函式庫 <https://github.com/OGRECave/ogre-next-deps>

把兩個 repo 放在同一層目錄下，比如像這樣：

```
C:/OgreSDK/ogre
C:/OgreSDK/ogredeps
```

## 安裝 CMake 建構系統

Ogre 用的建構系統是 [CMake][cmake]，這是我第一次用 CMake，花了一些時間才搞懂用法。

CMake 本身並不直接編譯程式。它的主要用法讓開發者寫 CMake 腳本(通常叫CMakeLists.txt)，然後依據腳本生成適合各平台的專案檔案。例如在 Windows 上會生成 Visual Studio 專案，在 Mac 上則會生成 Xcode 專案。

寫這篇文章的時候我安裝的是 CMake 3.17。到 [CMake][cmake] 官方網站下載安裝就好了。

[cmake]: https://cmake.org/  "CMake official site"

## 編譯依賴函式庫

首先，我們需要先編譯 `ogre-next-deps`，這裡面都是 Ogre3D 用到的第三方函式庫。

打開 **CMake-GUI**

![ogredeps](/img/cmake-ogredeps.png)

- **Where is the source code** 這一欄填寫 Repo 的目錄位置
- **Where to build the binaries** 這一欄通常按照慣例，會在原始碼目錄下加一層 build

接著以下步驟

- 按 **Configure**，選擇編譯器版本 `Visual Studio 16 2019 Win64`
- 再按 **Generate** ，產生 VS 專案檔
- 按 **Open Project** 打開 Visual Studio solution
- 開啟 Visual Studio 之後，編譯 `ALL_BUILD` 專案，Debug/Release 都要
- 接著單獨編譯 `INSTALL` 專案，在 INSTALL 上按右鍵 Build，同樣 Debug/Release 都要

這樣子第三方函式庫就算編譯完成了

## 編譯引擎本體

接著編譯 Ogre 引擎本體。跟前一步一樣，打開 CMake GUI

- **Where is the source code** 填 ogre 引擎的原始碼目錄
- **Where to build the binaries** 往下加一層子目錄 build

![](/img/cmake-ogre3d.png)

按下 Configure 按鈕之後呢，應該會跳出一些錯誤，別擔心，這是因為 CMake 無法定位前一步編譯的依賴庫的位置，藉由設定 **OGRE_DEPENDENCIES_DIR**  這個欄位告訴 CMake 依賴庫的位置：

- **OGRE_DEPENDENCIES_DIR** 欄位填入 `C:/OgreSDK/ogredeps/build/ogredeps`
- 渲染引擎可選用 DirectX 或者 OpenGL3+，兩者都勾也行
- OGRE_BUILD_SAMPLES2 是 Ogre 2.1 版的官方範例集，勾起來
- 勾選 SSE2 硬體指令集，加速數學運算
- `OGRE_UNITY_BUILD` 可以大大縮短編譯的時間，建議勾選

![](/img/cmake-ogre3d-config.png)

- 再 Congifure 一次，應該就沒有錯誤訊息了
- 按下 Generate 產生 Ogre.sln
- Open Project 打開 Visual Studio
- 編譯 `ALL_BUILD` 專案，Debug/Release 都要
- 編譯 `INSTALL` 專案，同樣 Debug/Release 都要

到此為止全部完成，挑選任一 Sample 執行即可

下圖是我成功執行 Ogre-Next 官方範例 Forwrad3D 的畫面：

![](/img/ogre3d-forward.jpg)

附註：

我寫這篇文章時的開發環境
- 作業系統 Windows 10 Home
- 編譯器 Visual Studio 2019 Community
- 版本控制系統 git
- 建構系統 CMake 3.17
