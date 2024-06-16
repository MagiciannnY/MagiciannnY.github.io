---
layout: post
title: "牛客周赛 Round 47"
data: 2024-06-16
tags: [acm]
comment: true
toc: false
author: Magiciannny
---

赛题链接：[牛客周赛 Round 47](https://ac.nowcoder.com/acm/contest/84851)

## B 茉茉的密码

只需找到一个公共字母即可。

## C 苗苗的气球

题目表述有点问题，如果气球一个都没了，就否认其他所有气球剩余的情况，输出 $0$。

特判：

1. 存在 $a_i$ 等于 $a$ 中其余元素的和，气球全部爆炸，输出 $0$ 
2. 存在 $a_i$ 大于 $a$ 中其余元素的和，那么只会剩下一种气球，即第 $i$ 种

对于每一种气球，考虑其余气球在一块的爆炸情况（其余气球总和为 $s$）：

1. $s\%2==0$ ：全部爆炸，剩余第 $i$ 种气球，输出 $1$ 
2. $s\%2==1$ ：只要第 $i$ 种气球数量大于 $1$，则会剩余，输出 $1$ 

## D 萌萌的好数

注意循环规律

## E 茜茜的计算器

满足轴对称形式的情况如下：

1. 由 $0, 1, 3, 8$ 组成的上下对称数
2. 由 $0, 2, 5, 8$ 组成的对称数，其中：
	- $n$ 为奇数时，$2, 5$ 不能在中间位
    - **数字若包含 $1$ 则不满足左右对称**

故而计算式如下：

$$
ans = 4^n + 4^{n/2} \times 2^{n\%2} - 2^{(n + 1)/2}
$$



C++代码如下：

``` c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int p = 1e9 + 7;

int n;
ll binPow(ll a, int k) {
    ll ans = 1;
    while (k) {
        if (k & 1) ans = ans * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return ans;
}

int main() {
    cin >> n;
    ll ans = binPow(4LL, n);
    ans = (ans + (binPow(4LL, n / 2) * binPow(2LL, n % 2) - binPow(2, (n + 1) / 2) + p) % p) % p;
    cout << ans << '\n';
    return 0;
}
```