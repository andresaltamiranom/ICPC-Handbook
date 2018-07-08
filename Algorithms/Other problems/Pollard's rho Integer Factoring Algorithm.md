# Pollard's rho Integer Factoring Algorithm

For a _fast_ integer factorization of integers up to ≈ 1 * 10^9 (64-bit unsigned integers), one can use Pollard's rho algorithm. The key idea of this algorithm is that two integers _x_ and _y_ are congruent modulo _p_ (_p_ is one of the factors of _n_, the integer that we want to factor) with probability of 0.5 after 'a few (1.177 √p) integers' have been randomly chosen.

The theoretical details of this algorithm is probably not that important for Competitive Programming. Following is a working C++ implementation which can be used to handle composite integers that fit in 64-bit unsigned integers in a contest environment. However, Pollard's rho cannot factor an integer _n_ if _n_ is a large prime due to the way the algorithm works. To handle this case, we have to implement a fast (probabilistic) prime testing like the Miller-Rabin's algorithm.

```cpp

#define abs_val(a) (((a)>0)?(a):-(a))
typedef long long ll;

ll mulmod(ll a, ll b, ll c) { // returns (a * b) % c, and minimize overflow
	ll x = 0, y = a % c;
	while (b > 0) {
		if (b % 2 == 1) x = (x + y) % c;
		y = (y * 2) % c;
		b /= 2;
	}
	return x % c;
}

ll gcd(ll a, ll b) { return !b ? a : gcd(b, a % b); } // standard gcd

ll pollard_rho(ll n) {
	int i = 0, k = 2;
	ll x = 3, y = 3; // random seed = 3, other values possible
	while (1) {
		i++;
		x = (mulmod(x, x, n) + n - 1) % n; // generating function
		ll d = gcd(abs_val(y - x), n); // the key insight
		if (d != 1 && d != n) return d; // found one non-trivial factor
		if (i == k) y = x, k *= 2;
	}
}

int main() {
	ll n = 2063512844981574047LL; // we assume that n is not a large prime
	ll ans = pollard_rho(n); // break n into two non trivial factors
	if (ans > n / ans) ans = n / ans; // make ans the smaller factor
	printf("%lld %lld\n", ans, n / ans);
} // return 0;

```

We can also implement Pollard's rho algorithm in Java and use the _isProbablePrime_ function in Java BigInteger class. This way, we can accept _n_ larger than (2^64) - 1, e.g. 17798655664295576020099, which is ≈ 2^74, and factor it into 143054969437 * 124418296927. However, the runtime of Pollard's rho algorithm increases with larger _n_. The fact that integer factoring is a very difficult task is still the key concept of modern cryptography.
