# Suffix Array

A _suffix array_ is an integer array that stores a permutation of _n_ indices of _sorted_ suffixes. It is a good data structure to use for many challenging string problems involving long strings.

_Suffix arrays_ are closely related to _suffix trees_. _Suffix arrays_ are simpler to construct and to use, especially in programming contest settings. However, while a _suffix tree_ is constructed in O(n) time, a _suffix array_ is constructed in O(nlog(n)) time.

For example, consider the string S = ’acacag$’ with _N_ = 7 (representing its length). The _suffix array_ of S is an integer permutation of _n_ as shown in the following figure. Note that we regard the terminating symbol '$' as the lexicographically
smallest character.

![alt text](https://i.imgur.com/FLHe7Vg.png)

As we can see in the following figure, the leaves of a _suffix tree_ (from left to right) is in _suffix array_ order. In short, a vertex in _suffix tree_ corresponds to a range in _suffix array_.

![alt text](https://i.imgur.com/3NDD2FD.png)

Following is an implementation of a suffix array.

```cpp

struct SuffixArray {
  vi sa, lcp;
  int N, Q = 1<<7;
  vector<int> csort(vector<ii> &val) {
    #define get(t, num) ((num) ? ((t).fi) : ((t).se))
    vi currentOrder(N, 0), nextOrder(N, 0);
    FOR(i, 0, N) currentOrder[i] = i;
    auto cur = &currentOrder, nex = &nextOrder;
    FOR(j, 0, 2) {
      vi freq(N, 0), rank(N, 0), count(N, 0);
      FOR(i, 0, N) freq[get(val[i], j)]++;
      for(int i = 0, k = 0; i < N; i++) rank[i] = k, k += freq[i];	
      FOR(i, 0, N) (*nex)[rank[get(val[(*cur)[i]], j)]++] = (*cur)[i];
      swap(cur, nex);
    }
    return *cur;
  }
  SuffixArray(char *S, int N) : N(N) {
    sa.assign(N, 0);
    FOR(i, 0, N) sa[i] = i;
    vi freq(Q, 0);
    int index[Q], rank[N], k = 0;
    FOR(i, 0, N) freq[S[i]]++;
    FOR(i, 0, Q) index[i] = k, k += freq[i];
    FOR(i, 0, N) rank[i] = index[S[i]];
    for(int len = 2; len <= N*2; len <<= 1) {
      vii val;
      FOR(i, 0, N) val.eb(rank[i], (i + len/2 >= N) ? 0 : rank[i + len/2]);
      vi order = csort(val);
      FOR(i, 0, N) rank[order[i]] = i && val[order[i]] == val[order[i-1]] ? rank[order[i-1]] : i;
    }
    FOR(i, 0, N) sa[rank[i]] = i;
    buildLCP(S);
  }
  void buildLCP(char *S) {
    vi phi(N), plcp(N);
    int L = 0;
    phi[sa[0]] = -1;
    FOR(i, 1, N) phi[sa[i]] = sa[i-1];
    FOR(i, 0, N) {
      if(phi[i] == -1) { plcp[i] = 0; continue; }
      while(S[i+L] == S[phi[i]+L]) L++;
      plcp[i] = L;
      L = max(L-1, int(0));
    }
    FOR(i, 0, N) lcp.pb(plcp[sa[i]]);
  }
};

```

The vector of integers _sa_ represents the suffix array, while _lcp_ represents the _Longest Common Prefix_ between consecutive suffixes in suffix array order (this will be explained further below in the _buildLCP()_ function explanation).

_N_ is the size of the suffix array, including the terminating symbol '$'.

Following is an explanation for each of its functions.

### SuffixArray

```cpp

SuffixArray(char *S, int N) : N(N) {
  sa.assign(N, 0);
  FOR(i, 0, N) sa[i] = i;
  vi freq(Q, 0);
  int index[Q], rank[N], k = 0;
  FOR(i, 0, N) freq[S[i]]++;
  FOR(i, 0, Q) index[i] = k, k += freq[i];
  FOR(i, 0, N) rank[i] = index[S[i]];
  for(int len = 2; len <= N*2; len <<= 1) {
    vii val;
    FOR(i, 0, N) val.eb(rank[i], (i + len/2 >= N) ? 0 : rank[i + len/2]);
    vi order = csort(val);
    FOR(i, 0, N) rank[order[i]] = i && val[order[i]] == val[order[i-1]] ? rank[order[i-1]] : i;
  }
  FOR(i, 0, N) sa[rank[i]] = i;
  buildLCP(S);
}

```

**Input:** A string with a terminating symbol and its size.

**Output:** None.

Suffix Array's constructor. //TODO
