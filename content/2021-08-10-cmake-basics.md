+++
date=2021-08-10
title="CMake 快速上手：跨平台 C++ 專案建置"
[taxonomies]
tags = ["CMake", "C++"]
+++

這篇文章紀錄我入門學習 CMake 的心得  

### 為什麼需要 CMake?

簡單講，因為 C++ 語言跨平台很麻煩。

C++ 程式碼本身是可以跨平台的，主流平台也都可以編譯 C++，但是每個平台編譯程式碼用的工具鍊不同，差別很大。Linux 用 Makefile，Windows 用 Visual Studio Project， macOS 上可以用 Xcode Project 也可以用 Makefile，這些格式都互不相通。所以即使我寫了通用的程式碼，跨平台編譯專案依然然困難重重。

針對這個問題，除了同時維護多個個專案之外，還可以考慮 CMake 這類工具。

CMake 的賣點就是代我們處理編譯工具鍊問題。我們可以寫一份完全中立、跟平台無關的 CMake 腳本，然後讓 CMake 作為中間人，代為操作當前平台的編譯工具鏈。當我想在 Windows 上編譯專案時，CMake 就產生一個 Visual Studio 專案。同理，當我要在 macOS 上編譯專案時，就產生 Xcode 專案。

我個人時常在 Windows 和 macOS 之間切換，偶爾需要跑一下 Linux。使用 CMake 可以節省我的時間精力，避開每個平台都再學習一次建構系統的成本。

## Hello CMake!

馬上開始寫第一個 CMake 專案。

CMake 規定，專案腳本的入口一定是個叫做 `CMakeLists.txt` 的純文字檔。所以我們在專案目錄下創建兩個檔案：`CMakeLists.txt` 和 `main.cpp`。

這是 `CMakeLists.txt` 的內容：
```cmake
cmake_minimum_required(VERSION 3.21) # 設定最低版本要求
project(HelloCMake)                  # 設定專案名稱
add_executable(MyHomework main.cpp)  # 指定執行檔和原始碼
```
這就是最簡單的 CMake 專案，只需要三行。這三行滿足了專案最基本的需求: 編譯 main.cpp 並產出執行檔 MyHomework.exe。

CMake 用的是自家的腳本語法，本文不會著墨太多在腳本語法上，一開始只要知道 `cmake_minimum_required()`、`project()`、`add_executable()` 這些是內建函數就行了，參數用空白分隔。

`add_executable()` 函數指定了執行檔和原始碼。在這個例子中，`MyHomework` 是執行檔名稱，`main.cpp` 是原始碼。可以有多個原始碼，檔名間用空白分隔。

井字號可以寫註解，讓腳本更容易閱讀。

`main.cpp`的內容就不多說了：
```cpp
#include <iostream>

int main() { std::cout << "Hello CMake!" << std::endl; return 0; }
```

## 編譯專案

接著，用以下命令指示 CMake 來編譯專案：

```bash
cmake -S . -B build # 產生當前平台專案檔
cmake --build build # 編譯專案
```

我們剛剛提過，CMake 不會自己編譯專案，而是產生當前平台的專案檔，然後再呼叫對應的編譯工具編譯。這兩行命令就是這個過程的兩個步驟。

`-S` 和 `-B` 是 CMake 的兩個參數，`-S` 參數指定專案的來源目錄，`-B` 參數指定編譯目錄。這樣 CMake 會在 build 目錄下產生對應的專案檔，然後用 `--build` 編譯專案。

這裡我們引入了一個新的概念，就是「來源目錄」和「編譯目錄」的區分。

來源目錄是指程式源碼的所在(也是 `CMakeLists.txt` 所在的目錄)。而編譯目錄則是用來放編譯產生的副產品的目錄，包括 CMake 替我們產生的當前平台專案，編譯暫存檔，以及最終編譯完成的執行檔。這些檔案是 CMake 和編譯器產生的，不是原始專案的一部分。

這樣做的好處是，分開編譯目錄和來源目錄，不會污染原始專案，比較好做版本控制。另外，也方便清理副產品。

依照 CMake 的慣例，編譯目錄通常是名為 build 的子目錄。在本例中，我們看一眼 build 子目錄，可以看見 CMake 為我產生的 VS2019 專案:
![CMake VS2019](/img/cmake-vs.png)

最後 `cmake --build build` 命令，就會驅動當前平台的工具練，然後實際編譯專案。在本例中，這個命令會呼叫 MSBuild 編譯專案。

當然，徑直打開 Visual Studio 來建構專案也可以，不一定要假手 CMake。

### 指定建構系統

除了完全交由 CMake 決定之外，也可以用 `cmake -G` 直接指定建構系統，例如 Visual Studio 2019、Makefiles、Xcode 等等。

`-G` 參數後面接的是建構系統的名稱，例如 `Visual Studio 17 2022` 就是 Visual Studio 2022，`Unix Makefiles` 就是 Makefiles，`Xcode` 就是 Xcode。 當然，你的電腦上要有對應的建構系統才行。完整的建構系統支援清單，請參考[這個CMake官方文件連結](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#manual:cmake-generators(7))

```bash
# 產生不同平台的專案的例子
cmake -G "Visual Studio 16 2019" -S /source -B build
cmake -G "Visual Studio 15 2017" -S /source -B build
cmake -G "Unix Makefiles"   -S /source -B build
cmake -G Xcode              -S /source -B build
cmake -G Ninja              -S /source -B build
```

### 指定 Debug/Release 編譯組態

開發時我們常常需要 Debug 和 Release 兩種編譯組態。

可用 `--config` 指定編譯組態
```bash
cmake --build . --config Release
cmake --build . --config Debug
```

注意，這個參數只對 Visual Studio 和 Xcode 這類可以切換 Debug/Release 的專案有用，對 Makefiles 無效。

## 比較完整的 C++ 專案範例

看完了上面的極簡三行，接著是一個比較完整的 CMake C++ 專案範例。在剛剛的基礎上，加入 C++ 專案常見的標準備配置：標頭檔、多個原碼檔案、指定 C++11/14/17 版本標準、第三方程式庫的 include 路徑和 linker 路徑等等。有了這些，應該足以應付大多數開發需求。

```cmake
cmake_minimum_required(VERSION 3.21)
project(MyProject)

# 要求 C++17 標準
set(CMAKE_CXX_STANDARD 17)
# 多個原始碼檔案和標頭檔
add_executable(MyApp main.cpp work.cpp header1.h header2.h)
# 設定 include 目錄
target_include_directories(MyApp PRIVATE "C:/path/to/include")
# 設定 lib 目錄
target_link_libraries(MyApp PRIVATE "C:/path/to/lib")
```

藉由 `target_include_directories()` 和 `target_link_libraries()` 函數，我們可以告訴編譯器，要去哪裡找第三方標頭檔和函式庫，記得要給絕對路徑。

## Win32 視窗程式

如果要開發 Win32 視窗程式，可以添加以下 CMake 設定：
```cmake
# 關鍵字 WIN32 指明是 win32 視窗程式
add_executable(MyWin32App WIN32 main.cpp work.cpp header.h pch.h)
# 設定寬字元
target_compile_definitions(MyWin32App PRIVATE UNICOD _UNICODE)
``` 
- 用 `WIN32` 關鍵字指明是 win32 視窗程式，這樣程式入口會從 `main()` 變成 `WinMain()`
- 定義 `UNICODE` 和 `_UNICODE` 告訴 VC++ 怎麼處理寬字元

## 參考連結

- [CMake 官方教學](https://cmake.org/cmake/help/latest/guide/tutorial/)
- [CLion Quick CMake tutorial](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html)
- [An introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/)
- [Qt5 Projects with CMake](https://gist.github.com/Rod-Persky/e6b93e9ee31f9516261b)
- [用CMake在建置時複製檔案到輸出執行檔的目錄 ][CopyShader]

[CopyShader]: https://minecraftxwinp.github.io/2017/11/27/%E7%94%A8CMake%E5%9C%A8%E5%BB%BA%E7%BD%AE%E6%99%82%E8%A4%87%E8%A3%BD%E6%AA%94%E6%A1%88%E5%88%B0%E8%BC%B8%E5%87%BA%E5%9F%B7%E8%A1%8C%E6%AA%94%E7%9A%84%E7%9B%AE%E9%8C%84/