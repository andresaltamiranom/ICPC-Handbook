# Lowest Common Ancestor

The lowest common ancestor (LCA) of two nodes of a rooted tree is the lowest node whose subtree contains both nodes. A typical problem is to efficiently process queries that ask to find the lowest common ancestor of two nodes.

**Example**

In the following tree, the lowest common ancestor of nodes 5 and 8 is node 2:

![LCA](https://i.imgur.com/FNccsEV.png)

Following is an implementation of LCA. The constructor receives the constructed graph and the root node of this graph. The _query()_ function receives two nodes and returns the LCA. This solution uses DFS for searching.

```cpp

struct LCA {
  vi order, height, index, st;
  int minIndex(int i, int j) {
    return height[i] < height[j] ? i : j;
  }
  LCA(Graph &g, ll root) {
    index.assign(g.V, -1);
    dfs(g, root, 0);
    st.assign(height.size()*2, 0);
    FOR(i, 0, height.size())
      st[height.size() + i] = i;
    for(int i = height.size()-1; i; i--)
      st[i] = minIndex(st[i<<1], st[i<<1|1]);
  }
  void dfs(Graph &g, ll cv, ll h) {
    index[cv] = order.size();
    order.pb(cv), height.pb(h);
    FORC(g.edges[cv], edge)
      if(index[edge->to] == -1) {
        dfs(g, edge->to, height.back() + 1);
        order.pb(cv), height.pb(h);
      }
  }
  ll query(ll i, ll j) {
    int from = index[i], to = index[j];
    if (from > to) swap(from, to);
    int idx = from;
    for(int l = from + height.size(), r = to + height.size() + 1; l < r; l >>= 1, r >>= 1) {
      if (l&1) idx = minIndex(idx, st[l++]);
      if (r&1) idx = minIndex(idx, st[--r]);
    }
    return order[idx];
  }
};

```
