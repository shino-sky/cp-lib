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


# :warning: tree/lca.cpp

<a href="../../index.html">Back to top page</a>

* category: <a href="../../index.html#c0af77cf8294ff93a5cdb2963ca9f038">tree</a>
* <a href="{{ site.github.repository_url }}/blob/master/tree/lca.cpp">View this file on GitHub</a>
    - Last commit date: 2020-05-14 23:32:59+09:00




## Depends on

* :question: <a href="../template.cpp.html">template.cpp</a>
* :warning: <a href="dfs.cpp.html">tree/dfs.cpp</a>
* :heavy_check_mark: <a href="../util/fix.cpp.html">util/fix.cpp</a>


## Code

<a id="unbundled"></a>
{% raw %}
```cpp
#pragma once

#include "template.cpp"
#include "tree/dfs.cpp"

struct LCA {
 private:
  const int n;
  const int log2_n;

 public:
  vector<vector<int>> parent;
  vector<int> depth;
  LCA(const Graph& g, int root = 0)
      : n(g.size()),
        log2_n(32 - __builtin_clz(n) + 1),
        parent(log2_n, vector<int>(n)),
        depth(n) {
    fix([&](auto f, int v, int p, int d) -> void {
      parent[0][v] = p;
      depth[v] = d;
      for (auto& e : g[v]) {
        if (e.to != p) f(e.to, v, d + 1);
      }
    })(root, -1, 0);
    rep(k, log2_n - 1) {
      rep(v, n) {
        if (parent[k][v] < 0)
          parent[k + 1][v] = -1;
        else
          parent[k + 1][v] = parent[k][parent[k][v]];
      }
    }
  }

 public:
  int operator()(int u, int v) {
    if (depth[u] > depth[v]) swap(u, v);
    for (int k = 0; k < log2_n; k++) {
      if ((depth[v] - depth[u]) >> k & 1) {
        v = parent[k][v];
      }
    }
    if (u == v) return u;
    for (int k = log2_n - 1; k >= 0; k--) {
      if (parent[k][u] != parent[k][v]) {
        u = parent[k][u];
        v = parent[k][v];
      }
    }
    return parent[0][u];
  }

  int dist(int u, int v) {
    return depth[u] + depth[v] - depth[(*this)(u, v)] * 2;
  }
};
```
{% endraw %}

<a id="bundled"></a>
{% raw %}
```cpp
#line 2 "tree/lca.cpp"

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


#line 2 "tree/dfs.cpp"

#line 2 "util/fix.cpp"

#line 4 "util/fix.cpp"

template <typename F>
class FixPoint : private F {
 public:
  explicit constexpr FixPoint(F&& f) : F(forward<F>(f)) {}

  template <typename... Args>
  constexpr decltype(auto) operator()(Args&&... args) const {
    return F::operator()(*this, forward<Args>(args)...);
  }
};

template <typename F> decltype(auto) fix(F&& f) noexcept {
  return FixPoint<F>{forward<F>(f)};
}
#line 5 "tree/dfs.cpp"

struct DFS {
  VI subtree_sz, par;
  VLL dist;
};

// tree
DFS dfs(const Graph& graph, int root = 0) {
  int n = graph.size();
  DFS res;
  res.subtree_sz = VI(n, 1);
  res.par = VI(n, -1);
  res.dist = VLL(n, INF_LL);
  res.dist[root] = 0;
  fix([&](auto f, auto v)->void{
    for (auto e : graph[v])
      if (e.to != res.par[v])
        res.dist[e.to] = res.dist[v] + e.cost,
        res.par[e.to] = v,
        f(e.to),
        res.subtree_sz[v] += res.subtree_sz[e.to];
  })(root);
  return res;
}
#line 5 "tree/lca.cpp"

struct LCA {
 private:
  const int n;
  const int log2_n;

 public:
  vector<vector<int>> parent;
  vector<int> depth;
  LCA(const Graph& g, int root = 0)
      : n(g.size()),
        log2_n(32 - __builtin_clz(n) + 1),
        parent(log2_n, vector<int>(n)),
        depth(n) {
    fix([&](auto f, int v, int p, int d) -> void {
      parent[0][v] = p;
      depth[v] = d;
      for (auto& e : g[v]) {
        if (e.to != p) f(e.to, v, d + 1);
      }
    })(root, -1, 0);
    rep(k, log2_n - 1) {
      rep(v, n) {
        if (parent[k][v] < 0)
          parent[k + 1][v] = -1;
        else
          parent[k + 1][v] = parent[k][parent[k][v]];
      }
    }
  }

 public:
  int operator()(int u, int v) {
    if (depth[u] > depth[v]) swap(u, v);
    for (int k = 0; k < log2_n; k++) {
      if ((depth[v] - depth[u]) >> k & 1) {
        v = parent[k][v];
      }
    }
    if (u == v) return u;
    for (int k = log2_n - 1; k >= 0; k--) {
      if (parent[k][u] != parent[k][v]) {
        u = parent[k][u];
        v = parent[k][v];
      }
    }
    return parent[0][u];
  }

  int dist(int u, int v) {
    return depth[u] + depth[v] - depth[(*this)(u, v)] * 2;
  }
};

```
{% endraw %}

<a href="../../index.html">Back to top page</a>
