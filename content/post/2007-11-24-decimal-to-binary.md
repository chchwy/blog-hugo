---
date: 2007-11-24
title: 十進位轉二進位
isCJKLanguage: true
tags: [C++]
---

目前為止寫過最簡短的版本

```cpp
#include <iostream>
#include <climits>
using namespace std;

int main()
{
    int x = 0;
    unsigned int y = INT_MIN;
    cout << "please enter a Integer:"
    cin >> x;
    while (y != 0)
    {
        (x & y) ? (cout << "1") : (cout << "0");
        y = y << 1;
    }
}
```

概念： INT_MIN 的二進位長這樣 10000000000000000000000000000000
跟 x 做 bit AND，除了最左邊bit，其他bit都會變0。

只有最左邊bit維持原樣，這時就可以單獨判斷該bit要印出0或1。
接下來將INT_MIN 往右shift一個bit。就可以判斷x第二個bit了，依此類推。