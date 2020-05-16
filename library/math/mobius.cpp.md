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


# :warning: math/mobius.cpp

<a href="../../index.html">Back to top page</a>

* category: <a href="../../index.html#7e676e9e663beb40fd133f5ee24487c2">math</a>
* <a href="{{ site.github.repository_url }}/blob/master/math/mobius.cpp">View this file on GitHub</a>
    - Last commit date: 2020-05-16 16:05:01+09:00




## Depends on

* :warning: <a href="prime.cpp.html">math/prime.cpp</a>
* :question: <a href="../template.cpp.html">template.cpp</a>


## Code

<a id="unbundled"></a>
{% raw %}
```cpp
#pragma once

#include "template.cpp"
#include "math/prime.cpp"

class Mobeus {
 private:
  static vector<int> mobeus;
  static Prime prime;

 public:
  Mobeus(int n = 0) { build(n); }

  void build(int n) {
    if (mobeus.size() > n) return;
    chmax(n, 1);
    prime.build(n);
    mobeus.resize(n+1);
    fill(all(mobeus), 1);
    for (int p = 2; p < n+1; p++) if (prime(p)) {
      int val = 1;
      for (int i = p; i < n+1; i += p) mobeus[i] = -mobeus[i];
      if (p < sqrt(n+1)+1)
        for (int i = p*p; i < n+1; i += p*p) mobeus[i] = 0;
    }
  }

  int operator()(int n) {
    build(n);
    return mobeus[n];
  }
};

vector<int> Mobeus::mobeus;
Prime Mobeus::prime;

```
{% endraw %}

<a id="bundled"></a>
{% raw %}
```cpp
#line 2 "math/mobius.cpp"

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
template <typename T, typename U> T divceil(T m, U d) { return (m + d - 1) / d; }
template <typename T> vector<T> make_v(size_t a, T b) { return vector<T>(a, b); }
template <typename... Ts> auto make_v(size_t a, Ts... ts) {
  return vector<decltype(make_v(ts...))>(a, make_v(ts...));
}

// debugging stuff
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wmisleading-indentation"
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
#ifdef LOCAL
#define dump(...) debugos << "  " << string(#__VA_ARGS__) << ": " << "[" << to_string(__LINE__) \
<< ":" << __FUNCTION__ << "]" << newl << "    ", dump_func(__VA_ARGS__)
#else
#define dump(...)
#endif
#pragma GCC diagnostic pop


#line 2 "math/prime.cpp"

#line 4 "math/prime.cpp"

class Prime {
 private:
  static vector<bool> prime;

 public:
  Prime(int n = 0) { build(n); }

  void build(int n) {
    if (prime.size() > n) return;
    n *= 2;
    chmax(n, 1);
    prime.resize(n+1);
    fill(all(prime), true);
    prime[0] = prime[1] = false;
    rep(i, 2, n+1) if (prime[i]) {
      for (int j = i*2; j < n+1; j += i) prime[j] = false;
    }
  }

  bool operator()(int n) {
    build(n);
    return prime[n];
  }

  static vector<int> primes(int mx) {
    Prime prime(mx);
    vector<int> res;
    rep(n, 2, mx+1) if (prime(n)) res.push_back(n);
    return res;
  }

  static bool is_prime(int n) {
    if (n == 1) return false;
    if (n == 2) return true;
    rep(d, 2, sqrt(n)+1) if (n % d == 0) return false;
    return true;
  }
};

vector<bool> Prime::prime;
#line 5 "math/mobius.cpp"

class Mobeus {
 private:
  static vector<int> mobeus;
  static Prime prime;

 public:
  Mobeus(int n = 0) { build(n); }

  void build(int n) {
    if (mobeus.size() > n) return;
    chmax(n, 1);
    prime.build(n);
    mobeus.resize(n+1);
    fill(all(mobeus), 1);
    for (int p = 2; p < n+1; p++) if (prime(p)) {
      int val = 1;
      for (int i = p; i < n+1; i += p) mobeus[i] = -mobeus[i];
      if (p < sqrt(n+1)+1)
        for (int i = p*p; i < n+1; i += p*p) mobeus[i] = 0;
    }
  }

  int operator()(int n) {
    build(n);
    return mobeus[n];
  }
};

vector<int> Mobeus::mobeus;
Prime Mobeus::prime;

```
{% endraw %}

<a href="../../index.html">Back to top page</a>
