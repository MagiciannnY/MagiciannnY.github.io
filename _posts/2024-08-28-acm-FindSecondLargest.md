---
layout: post
title: "The Number of Second Largest Value"
data: 2024-08-28
tags: acm SegmentTree
comment: true
toc: false
author: Magiciannny
---

# 区间查询第二大的数的个数

模板链接：[F - Second Largest Query](https://atcoder.jp/contests/abc343/tasks/abc343_f) 

线段树维护四个值：最大值及其数目，次大值及其数目

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 2e5 + 5;

#define all(x) x.begin(), x.end()
#define pii pair<int, int>
#define fi first
#define se second
#define mk make_pair
#define pb push_back

struct info {
    int s1, s2, cnt1, cnt2;
    info operator + (const info& rhs) const {
        info res;
        map<int, int> mp;
        if (s1) mp[s1] += cnt1;
        if (s2) mp[s2] += cnt2;
        if (rhs.s1) mp[rhs.s1] += rhs.cnt1;
        if (rhs.s2) mp[rhs.s2] += rhs.cnt2;
        vector<pii> t;
        for (auto& [k, v] : mp) {
            t.pb(mk(k, v));
        }
        auto cmp = [&](pii a, pii b) -> bool {
            return a.fi > b.fi;
        };
        sort(all(t), cmp);
        res.s1 = t[0].fi; res.cnt1 = t[0].se;
        if (t.size() > 1) {res.s2 = t[1].fi; res.cnt2 = t[1].se;}
        else {res.s2 = 0, res.cnt2 = 0;}
        return res;
    }
    void print() {
        cout << s1 << " " << cnt1 << " " << s2 << " " << cnt2 << '\n';
    }
};

int a[N];

struct Segment {
    #define lson n << 1
    #define rson n << 1 | 1
    info d[N << 2];
    void build(int l, int r, int n) {
        if (l == r) {
            d[n] = {a[l], 0, 1, 0};
            return;
        }
        int m = (l + r) >> 1;
        build(l, m, lson);
        build(m + 1, r, rson);
        d[n] = d[lson] + d[rson];
    }
    void update(int pos, int w, int l, int r, int n) {
        if (l > pos || r < pos) return;
        if (l == r && l == pos) {
            d[n].s1 = w;
            return;
        }
        int m = (l + r) >> 1;
        update(pos, w, l, m, lson);
        update(pos, w, m + 1, r, rson);
        d[n] = d[lson] + d[rson];
    }
    info query(int ql, int qr, int l, int r, int n) {
        if (ql <= l && qr >= r) {
            return d[n];
        }
        int m = (l + r) >> 1;
        if (qr <= m) return query(ql, qr, l, m, lson);
        if (ql > m) return query(ql, qr, m + 1, r, rson);
        return query(ql, m, l, m, lson) + query(m + 1, qr, m + 1, r, rson);
    }
} seg;

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    int n, q; cin >> n >> q;
    for (int i = 1;i <= n;i++) {
        cin >> a[i];
    }
    seg.build(1, n, 1);
    while (q--) {
        int op; cin >> op;
        if (op == 1) {
            int pos, x; cin >> pos >> x;
            if (a[pos] == x) continue;
            a[pos] = x;
            seg.update(pos, x, 1, n, 1);
        }
        else {
            int l, r; cin >> l >> r;
            cout << seg.query(l, r, 1, n, 1).cnt2 << '\n';
        }
    }
}
```

