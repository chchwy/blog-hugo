---
date: 2016-03-08
title: 避免 UVa 解題時手動輸入測資
tags: [C++, UVa Oneline Judge]
isCJKLanguage: true
---

解 ACM 題目的時候，通常測試資料都是走標準輸入 (stdin) 如 `cin/scanf()/gets()`，需要手動鍵入。測試資料量大的時候，手動鍵入測試資料非常浪費時間。我後來發現了一個技巧叫做 I/O 轉向 (I/O redirection) ，可以重新定義標準輸入流，省去手動鍵入測資的步驟。

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
這一行的作用就是把 input.txt 當作 stdin 的資料來源。一旦呼叫過 `freopen()`，接下來所有的標準輸入函數 `cin/scanf()/getline()` 就變成從 input.txt 讀取資料。

```cpp
freopen("output.txt", "w", stdout);
```

同樣的也可以把標準輸出導向檔案寫出，這樣所有的 `cout/printf()/puts()` 都會寫入 output.txt 這個檔案。

因為 UVa online judge 在編譯的時候會下 `-DONLINE_JUDGE` 的編譯參數。所以我們把freopen() 用 `#ifndef` (if Not defined) 夾起來，這樣只有在本機測試的時候才做 I/O 轉向，上傳程式碼的時候走回正常的標準輸入。

