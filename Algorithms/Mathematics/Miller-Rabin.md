# Miller-Rabin

The Miller–Rabin primality test is an algorithm which determines whether a given number is prime. This method is a probabilistic method (like Fermat), but it is generally preferred over Fermat’s method.

Below are some important facts behind this algorithm:

1) Fermat’s theorem states that: If _n_ is a prime number, then for every _a_, _1 <= a < n_, _a^(n-1) % n = 1_
2) Base cases make sure that _n_ must be odd. Since _n_ is odd, _n-1_ must be even. And an even number can be written as _d * 2^s_ where _d_ is an odd number and _s > 0_.
3) From the above two points, for every randomly picked number in range _[2, n-2]_, the value of _a^(d*2^r) % n_ must be 1.
4) As per Euclid’s Lemma, if _x^2 % n = 1_ or _(x^2 – 1) % n = 0_ or _(x-1)(x+1) % n = 0_, then, for _n_ to be prime, either _n_ divides _(x-1)_ or _n_ divides _(x+1)_. Which means either _x % n = 1_ or _x % n = -1_.

From points 2 and 3, we can conclude:

    For n to be prime, either
    a^d % n = 1
         OR 
    a^(d*2^i) % n = -1
    for some i, where 0 <= i <= r-1
	
Following is an implementation of this method that runs in O(n^3) time.

```cpp

ll mulmod(ll a, ll b, ll c) {
  ll x = 0, y = a % c;
  while (b) {
    if (b & 1) x = (x + y) % c;
    y = (y << 1) % c;
    b >>= 1;
  }
  return x % c;
}

ll fastPow(ll x, ll n, ll MOD) {
  ll ret = 1;
  while (n) {
    if (n & 1) ret = mulmod(ret, x, MOD);
    x = mulmod(x, x, MOD);
    n >>= 1;
  }
  return ret;
}

bool isPrime(ll n) {
  ll d = n - 1;
  int s = 0;
  while (d % 2 == 0) {
    s++;
    d >>= 1;
  }
  // It's guaranteed that these values will work for any number smaller than 3*10^18 (a 3 followed by 18 zeros)
  int a[9] = { 2, 3, 5, 7, 11, 13, 17, 19, 23 };
  FOR(i, 0, 9) {
    bool comp = fastPow(a[i], d, n) != 1;
    if(comp) FOR(j, 0, s) {
      ll fp = fastPow(a[i], (1LL << (ll)j)*d, n);
      if (fp == n - 1) {
        comp = false;
        break;
      }
    }
    if(comp) return false;
  }
  return true;
}

```
