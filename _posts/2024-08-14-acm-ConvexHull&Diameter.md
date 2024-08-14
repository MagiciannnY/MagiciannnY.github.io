---
layout: post
title: "ConvexHull and Diameter of Convex Polygon"
data: 2024-08-14
tags: acm 计算几何
comment: true
toc: false
author: Magiciannny
---

## 凸包问题以及凸多边形的直径

模板链接：[P1452 【模板】旋转卡壳](https://www.luogu.com.cn/problem/P1452) 

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
template <typename T>
struct Point {
    T x, y;
    // 构造函数
    Point() : x(0), y(0) {}
    template <typename A, typename B>
    Point(A x, B y) : x(x), y(y) {}
    // pair转Point
    template <typename A, typename B>
    Point(pair<A, B> p) : x(p.first), y(p.second) {}
    // 重载运算符
    Point operator+=(const Point p) {
        x += p.x, y += p.y;
        return *this;
    }
    Point operator-=(const Point p) {
        x -= p.x, y -= p.y;
        return *this;
    }
    Point operator+(Point p) const { return {x + p.x, y + p.y}; }
    Point operator-(Point p) const { return {x - p.x, y - p.y}; }
    bool operator==(Point p) const { return x == p.x && y == p.y; }
    bool operator!=(Point p) const { return x != p.x || y != p.y; }
    Point operator-() const { return {-x, -y}; }           // 关于中心对称
    Point operator*(T t) const { return {x * t, y * t}; }  // 数乘运算
    Point operator/(T t) const { return {x / t, y / t}; }
    // 大小比较
    bool operator<(Point p) const {
        if (x != p.x)
            return x < p.x;
        return y < p.y;
    }
    // 点乘
    T dot(Point other) { return x * other.x + y * other.y; }
    // 叉乘
    T det(Point other) { return x * other.y - y * other.x; }
    // 平方和开根
    double norm() { return sqrtl(x * x + y * y); }
    double norm2() { return x * x + y * y; }
};
// 传入下标从零开始的点数组，返回凸包数组
template <typename T>
vector<Point<T>> GrahamScan(vector<Point<T>>& p) {
    sort(p.begin(), p.end());
    int n = p.size(), m = 0;
    vector<Point<T>> res(n + 1);
    for (int i = 0; i < n; i++) {
        while (m > 1 && !((res[m - 1] - res[m - 2]).det(p[i] - res[m - 2]) > 0))
            m--;
        res[m++] = p[i];
    }
    int kk = m;
    for (int i = n - 2; i >= 0; i--) {
        while (m > kk && !((res[m - 1] - res[m - 2]).det(p[i] - res[m - 2]) > 0))
            m--;
        res[m++] = p[i];
    }
    // 凸包有m个顶点
    if (n > 1) m--;
    res.erase(res.begin() + m, res.end());
    return res;
}
// 求凸包直径，需要传入凸包数组，Point为(x, y)形式
template <typename T>
ll find_diameter(vector<Point<T>>& XY) {
    XY.push_back(XY[0]);
    ll mx = 0;                // 最大值
    int top = XY.size() - 1;  // top = 点数 + 1
    int j = 2;
    if (top < 3)
        return (XY[0] - XY[1]).norm2();
    for (int i = 0; i < top; i++) {
        while (abs((XY[i + 1] - XY[i]).det(XY[j] - XY[i])) <= abs((XY[i + 1] - XY[i]).det(XY[(j + 1) % top] - XY[i])))
            j = (j + 1) % top;
        ll d1 = (XY[i + 1] - XY[j]).norm2(), d2 = (XY[i] - XY[j]).norm2();
        mx = max(mx, max(d1, d2));
    }
    return mx;
}
using P = Point<ll>;
int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    int n, x, y;
    cin >> n;
    vector<P> G(n);
    for (int i = 0; i < n; i++) {
        cin >> G[i].x >> G[i].y;
    }
    vector<P> D = GrahamScan(G);
    ll ans = find_diameter(D);
    cout << ans << '\n';
}
```



