# Suffix Array

_Suffix arrays_ are closely related to _suffix trees_. Building an efficient _suffix tree_ under contest environment is a bit complex and risky. Fortunately, another data structure called _suffix array_ has similar functionalities as a _suffix tree_, but it is much simpler to implement, especially in programming contest settings. However, while a _suffix tree_ is constructed in O(n) time, a _suffix array_ is constructed in O(nlog(n)) time. _Suffix arrays_ are good enough for many challenging string problems involving **long strings** in programming contests.

A _suffix array_ is an integer array that stores a permutation of _n_ indices of _sorted_ suffixes.

For example, consider the string S = ’acacag$’ with _N_ = 7 (representing its length). The _suffix array_ of S is an integer permutation of _n_ as shown in the following figure. Note that we regard the terminating symbol '$' as the lexicographically
smallest character.

![alt text](https://i.imgur.com/FLHe7Vg.png)

As we can see in the following figure, the leaves of a _suffix tree_ (from left to right) is in _suffix array_ order. In short, a vertex in _suffix tree_ corresponds to a range in _suffix array_.

![alt text](https://i.imgur.com/3NDD2FD.png)

A _suffix array_ can be used to solve many string problems such as computing the LCP (Longest Common Prefix) of given strings. The LCP for a pair of strings _S1_ and _S2_ is the longest string _S_ which is the prefix of both _S1_ and _S2_. In this case, we can compute the LCP between consecutive suffixes in suffix array order. To compute the LCP, we use the Permuted Longest Common Prefix (PLCP) theorem, which states that it is easier to compute the LCP in the original position order of the suffixes instead of the lexicographic order of the suffixes. This method guarantees that the LCP will be computed in at most O(n) time.

Using the built LCP, we can solve other problems such as finding the Longest Repeated Substring in O(n) time or finding the Longest Common Substring in O(n) time.

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

The vector of integers _sa_ represents the suffix array and _N_ is the size of the suffix array, including the terminating symbol '$'.


