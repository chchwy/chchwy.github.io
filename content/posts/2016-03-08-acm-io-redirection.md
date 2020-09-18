---
date: 2016-03-08
title: UVa 解題自動輸入測資
tags: [C++, 程式解題, UVa Oneline Judge]
isCJKLanguage: true
---

解 ACM 題目的時候，走標準輸入 `cin/scanf()/gets()` 手動鍵入大量測試資料量不只煩人，還非常緩慢。後來我發現了一個技巧叫做 I/O 轉向 (I/O redirection) ，可以重新定義標準輸入流，省下大量手動鍵入測資的時間。

先上例子：
```cpp
int main() {

#ifndef ONLINE_JUDGE
  freopen("input.txt", "r", stdin);
  freopen("output.txt", "w", stdout);
#endif

  // do whatever you need to do
  // cin/cout/scanf/printf as usual

  return 0;
}
```

關鍵是 `freopen()` 這個函數。

```cpp
freopen("input.txt", "r", stdin);
```

透過 `freopen()` 可以把 `stdin` 的輸入源從鍵盤改成檔案。所以預先把測資寫在檔案 input.txt 裡，接下來的標準輸入函數 `cin/scanf()/getline()` 就會變成從 input.txt 讀取資料，非常方便。

```cpp
freopen("output.txt", "w", stdout);
```

同樣的也可以把標準輸出導向檔案，這樣所有的標準輸出 `cout/printf()/puts()` 都會寫入 output.txt 這個檔案。

接著把這兩個 `freopen()` 轉向程式碼用 `#ifndef ONLINE_JUDGE ... #endif` 包起來。因為 UVa online judge 編譯程式碼的時候有下編譯參數 `-DONLINE_JUDGE`。這樣子I/O 轉向在本機測試的時候才有作用，提交程式碼的時候依然走正常的標準輸入。

