---
layout: default
---

<!-- mathjax config similar to math.stackexchange -->
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: { equationNumbers: { autoNumber: "AMS" }},
    tex2jax: {
      inlineMath: [ ['$','$'] ],
      processEscapes: true
    },
    "HTML-CSS": { matchFontHeight: false },
    displayAlign: "left",
    displayIndent: "2em"
  });
</script>

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jquery-balloon-js@1.1.2/jquery.balloon.min.js" integrity="sha256-ZEYs9VrgAeNuPvs15E39OsyOJaIkXEEt10fzxJ20+2I=" crossorigin="anonymous"></script>
<script type="text/javascript" src="../../assets/js/copy-button.js"></script>
<link rel="stylesheet" href="../../assets/css/copy-button.css" />


# :heavy_check_mark: data-structure/segtree.cpp

<a href="../../index.html">Back to top page</a>

* category: <a href="../../index.html#36397fe12f935090ad150c6ce0c258d4">data-structure</a>
* <a href="{{ site.github.repository_url }}/blob/master/data-structure/segtree.cpp">View this file on GitHub</a>
    - Last commit date: 2020-05-14 23:32:59+09:00




## Depends on

* :question: <a href="../template.cpp.html">template.cpp</a>
* :question: <a href="../util/function-objects.cpp.html">util/function-objects.cpp</a>


## Required by

* :heavy_check_mark: <a href="../string/hash-monoid.cpp.html">string/hash-monoid.cpp</a>


## Verified with

* :heavy_check_mark: <a href="../../verify/verify/aoj/0355.test.cpp.html">verify/aoj/0355.test.cpp</a>
* :heavy_check_mark: <a href="../../verify/verify/aoj/0367.test.cpp.html">verify/aoj/0367.test.cpp</a>


## Code

<a id="unbundled"></a>
{% raw %}
```cpp
#pragma once

#include "template.cpp"
#include "util/function-objects.cpp"

#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wshadow"
template <typename T, typename Combine, typename Action>
class SegmentTree {
 private:
  const size_t n;
  const T unit;
  const Combine combine;
  const Action action;
  vector<T> data;

 public:
  SegmentTree(size_t n = 0, T unit = {}, Combine combine = {},
              Action action = {})
      : n(n), unit(unit), combine(combine), action(action), data(n << 1, unit) {
    build();
  }

  template <
      typename Iter,
      enable_if_t<is_same<typename Iter::value_type, T>::value>* = nullptr>
  SegmentTree(Iter first, Iter last, size_t n, T unit = {},
              Combine combine = {}, Action action = {})
      : n(n), unit(unit), combine(combine), action(action), data(n << 1) {
    copy(first, last, data.begin() + n);
    build();
  }

  template <
      typename Iter,
      enable_if_t<!is_same<typename Iter::value_type, T>::value>* = nullptr>
  [[deprecated]] SegmentTree(Iter first, Iter last, size_t n, T unit = {},
                             Combine combine = {}, Action action = {})
      : n(n), unit(unit), combine(combine), action(action), data(n << 1) {
    copy(first, last, data.begin() + n);
    build();
  }

  template <typename Iter>
  SegmentTree(Iter first, Iter last, T unit = {}, Combine combine = {},
              Action action = {})
      : SegmentTree(first, last, distance(first, last), unit, combine, action) {
  }

 private:
  void build() { repr(i, n) data[i] = combine(data[i << 1], data[i << 1 | 1]); }

 public:
  void modify(int l, T v) {
    l += n;
    data[l] = action(data[l], v);
    for (; l > 1; l >>= 1) data[l >> 1] = combine(data[l & (~1)], data[l | 1]);
  }

  T fold(int l, int r) const {
    if (l == r) return unit;
    if (l + 1 == r) return data[l + n];
    T resl = data[l += n], resr = data[(r += n) - 1];
    for (l++, r--; l < r; l >>= 1, r >>= 1) {
      if (l & 1) resl = combine(resl, data[l++]);
      if (r & 1) resr = combine(data[--r], resr);
    }
    return combine(resl, resr);
  }
};
#pragma GCC diagnostic pop

```
{% endraw %}

<a id="bundled"></a>
{% raw %}
```cpp
#line 2 "data-structure/segtree.cpp"

#line 2 "template.cpp"

#ifndef LOCAL
#pragma GCC diagnostic warning "-w"
#pragma GCC optimize("O3")
#pragma GCC optimize("unroll-loops")
#pragma GCC target("avx")
#endif
#include <algorithm>
#include <bitset>
#include <cassert>
#include <cmath>
#include <functional>
#include <iostream>
#include <map>
#include <numeric>
#include <queue>
#include <set>
#include <stack>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using VI = vector<int>;
using VVI = vector<vector<int>>;
using VLL = vector<ll>;
using VVLL = vector<vector<ll>>;
using VB = vector<bool>;
using PII = pair<int, int>;
using PLL = pair<ll, ll>;
constexpr int INF = 1000000007;
constexpr ll INF_LL = 1'000'000'000'000'000'007;
#define all(x) begin(x), end(x)
#define rall(x) rbegin(x), rend(x)
#define newl '\n'

// loops rep(until) / rep(var, until) / rep(var, from, until) / repr (reversed order)
#define OVERLOAD3(_1, _2, _3, name, ...) name
#define rep(...) OVERLOAD3(__VA_ARGS__, REPEAT_FROM_UNTIL, REPEAT_UNTIL, REPEAT)(__VA_ARGS__)
#define REPEAT(times) REPEAT_CNT(_repeat, __COUNTER__, times)
#define REPEAT_CNT(_repeat, cnt, times) REPEAT_CNT_CAT(_repeat, cnt, times)
#define REPEAT_CNT_CAT(_repeat, cnt, times) REPEAT_FROM_UNTIL(_repeat ## cnt, 0, times)
#define REPEAT_UNTIL(name, times) REPEAT_FROM_UNTIL(name, 0, times)
#define REPEAT_FROM_UNTIL(name, from, until) for (int name = from, name ## __until = (until); name < name ## __until; name++)
#define repr(...) OVERLOAD3(__VA_ARGS__, REPR_FROM_UNTIL, REPR_UNTIL, REPEAT)(__VA_ARGS__)
#define REPR_UNTIL(name, times) REPR_FROM_UNTIL(name, 0, times)
#define REPR_FROM_UNTIL(name, from, until) for (int name = (until)-1, name ## __from = (from); name >= name ## __from; name--)

template <typename T, typename U>
bool chmin(T& var, U x) { if (var > x) { var = x; return true; } else return false; }
template <typename T, typename U>
bool chmax(T& var, U x) { if (var < x) { var = x; return true; } else return false; }
ll power(ll e, ll t, ll mod = INF_LL) {
  ll res = 1; for (; t; t >>= 1, (e *= e) %= mod) if (t & 1) (res *= e) %= mod; return res;
}
ll choose(ll n, int r) {
  chmin(r, n-r); if (r < 0) return 0; ll res = 1; rep(i, r) res *= n-i, res /= i+1; return res;
}
template <typename T> T divceil(T m, T d) { return (m + d - 1) / d; }
template <typename T> vector<T> make_v(size_t a, T b) { return vector<T>(a, b); }
template <typename... Ts> auto make_v(size_t a, Ts... ts) {
  return vector<decltype(make_v(ts...))>(a, make_v(ts...));
}

// debug stuff
#define repi(it, ds) for (auto it = ds.begin(); it != ds.end(); it++)
class DebugPrint { public: template <typename T> DebugPrint& operator <<(const T& v) {
#ifdef LOCAL
    cerr << v;
#endif
return *this; } } debugos; template <typename T> DebugPrint& operator<<(DebugPrint& os, const
vector<T>& vec) { os << "{"; for (int i = 0; i < vec.size(); i++) os << vec[i] << (i + 1 ==
vec.size() ? "" : ", "); os << "}"; return os; } template <typename T, typename U> DebugPrint&
operator<<(DebugPrint& os, map<T, U>& map_var) { os << "{"; repi(itr, map_var) { os << *itr;
itr++; if (itr != map_var.end()) os << ", "; itr--; } os << "}"; return os; } template <
typename T> DebugPrint& operator<<(DebugPrint& os, set<T>& set_var) { os << "{"; repi(itr,
set_var) { os << *itr; itr++; if (itr != set_var.end()) os << ", "; itr--; } os << "}"; return
os; } template <typename T, typename U> DebugPrint& operator<<(DebugPrint& os, const pair<T, U
>& p) { os << "(" << p.first << ", " << p.second << ")"; return os; } void dump_func() {
debugos << newl; } template <class Head, class... Tail> void dump_func(Head &&head, Tail &&...
tail) { debugos << head; if (sizeof...(Tail) > 0) { debugos << ", "; } dump_func(std::move(
tail)...); }
#define dump(...) debugos << "  " << string(#__VA_ARGS__) << ": " << "[" << to_string(__LINE__) \
<< ":" << __FUNCTION__ << "]" << newl << "    ", dump_func(__VA_ARGS__)
#pragma GCC diagnostic pop


#line 2 "util/function-objects.cpp"

#line 4 "util/function-objects.cpp"

struct minT {
  template <typename T>
  T operator()(T a, T b) const {
    return min(a, b);
  }
};

struct maxT {
  template <typename T>
  T operator()(T a, T b) const {
    return max(a, b);
  }
};

#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wunused-parameter"
struct assignT {
  template <typename T>
  T operator()(T a, T b, int k = 0) const { return b; }
};
#pragma GCC diagnostic pop

struct plusT {
  template <typename T>
  T operator()(T a, T b, int k) const { return a + b * k; }
};
#line 5 "data-structure/segtree.cpp"

#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wshadow"
template <typename T, typename Combine, typename Action>
class SegmentTree {
 private:
  const size_t n;
  const T unit;
  const Combine combine;
  const Action action;
  vector<T> data;

 public:
  SegmentTree(size_t n = 0, T unit = {}, Combine combine = {},
              Action action = {})
      : n(n), unit(unit), combine(combine), action(action), data(n << 1, unit) {
    build();
  }

  template <
      typename Iter,
      enable_if_t<is_same<typename Iter::value_type, T>::value>* = nullptr>
  SegmentTree(Iter first, Iter last, size_t n, T unit = {},
              Combine combine = {}, Action action = {})
      : n(n), unit(unit), combine(combine), action(action), data(n << 1) {
    copy(first, last, data.begin() + n);
    build();
  }

  template <
      typename Iter,
      enable_if_t<!is_same<typename Iter::value_type, T>::value>* = nullptr>
  [[deprecated]] SegmentTree(Iter first, Iter last, size_t n, T unit = {},
                             Combine combine = {}, Action action = {})
      : n(n), unit(unit), combine(combine), action(action), data(n << 1) {
    copy(first, last, data.begin() + n);
    build();
  }

  template <typename Iter>
  SegmentTree(Iter first, Iter last, T unit = {}, Combine combine = {},
              Action action = {})
      : SegmentTree(first, last, distance(first, last), unit, combine, action) {
  }

 private:
  void build() { repr(i, n) data[i] = combine(data[i << 1], data[i << 1 | 1]); }

 public:
  void modify(int l, T v) {
    l += n;
    data[l] = action(data[l], v);
    for (; l > 1; l >>= 1) data[l >> 1] = combine(data[l & (~1)], data[l | 1]);
  }

  T fold(int l, int r) const {
    if (l == r) return unit;
    if (l + 1 == r) return data[l + n];
    T resl = data[l += n], resr = data[(r += n) - 1];
    for (l++, r--; l < r; l >>= 1, r >>= 1) {
      if (l & 1) resl = combine(resl, data[l++]);
      if (r & 1) resr = combine(data[--r], resr);
    }
    return combine(resl, resr);
  }
};
#pragma GCC diagnostic pop

```
{% endraw %}

<a href="../../index.html">Back to top page</a>
