# Lowest Common Ancestor

The lowest common ancestor (LCA) of two nodes of a rooted tree is the lowest node whose subtree contains both nodes. A typical problem is to efficiently process queries that ask to find the lowest common ancestor of two nodes.

**Example**

In the following tree, the lowest common ancestor of nodes 5 and 8 is node 2:

![LCA](https://i.imgur.com/FNccsEV.png)

To solve this, we traverse the tree using depth-first search:

![LCA](https://i.imgur.com/APA0dI9.png)

Then we add each node to the array when the depth-first search walks through the node. Hence, a node that has _k_ children appears _k + 1_ times in the array and there are a total of _2n−1_ nodes in the array.

We store two values: the identifier of the node and the depth/height of the node in the tree. The following arrays corresponds to the above tree:

![LCA](https://i.imgur.com/1XUNQAX.png)

Now we can find the lowest common ancestor of nodes _a_ and _b_ by finding the node with the minimum depth between nodes _a_ and _b_ in the array. For example, the lowest common ancestor of nodes 5 and 8 can be found as follows:

![LCA](https://i.imgur.com/2TKCrox.png)

Node 5 is at position 2, node 8 is at position 5, and the node with minimum depth between positions 2 to 5 is node 2 at position 3, whose depth is 2. Thus, the lowest common ancestor of nodes 5 and 8 is node 2. This means that to find the lowest common ancestor of two nodes it suffices to process a range minimum query. Since the array is static, we can process such queries in O(1) time after an O(nlog(n)) time preprocessing.

Following is an implementation of LCA. The constructor receives the constructed graph and the root node of this graph. The _query()_ function receives two nodes and returns the LCA.

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
