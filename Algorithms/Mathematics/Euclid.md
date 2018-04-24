# Euclid

Euclid's algorithm is an efficient method for computing the greatest common divisor (GCD) of two numbers, the largest number that divides both of them without leaving a remainder.

Following are some functions related to Euclid's.

## Helper functions

### Greatest Common Divisor

```cpp

int gcd(int a, int b) {
	int tmp;
	while(b){a%=b; tmp=a; a=b; b=tmp;}
	return a;
}
```

### Least Common Multiple

```cpp

int lcm(int a, int b) {
	return a/gcd(a,b)*b;
}
```

## Euclid functions

### Extended Euclid

```cpp

int extended_euclid(int a, int b, int &x, int &y) {  
	int xx = y = 0;
	int yy = x = 1;
	while (b) {
		int q = a/b;
		int t = b; b = a%b; a = t;
		t = xx; xx = x-q*xx; x = t;
		t = yy; yy = y-q*yy; y = t;
	}
	return a;
}

```
The **Extended Euclidean algorithm** is an extension to the Euclidean algorithm and computes, in addition to the GCD of integers _a_ and _b_, the coefficients of Bézout's identity, which are integers _x_ and _y_ such that

_ax+by = gcd(a,b)_

This is a certifying algorithm (an algorithm that outputs, together with a solution to the problem it solves, a proof that the solution is correct), because the GCD is the only number that can simultaneously satisfy this equation and divide the inputs. It allows one to also compute, with almost no extra cost, the quotients of _a_ and _b_ by their GCD.

Finds _x_, _y_ such that _d = ax + by_.

Returns _d = gcd(a,b)_.

### Modular Linear Equation Solver

```cpp

vi modular_linear_equation_solver(int a, int b, int n) {
	int x, y;
	vi solutions;
	int d = extended_euclid(a, n, x, y);
	if (!(b%d)) {
		x = mod (x*(b/d), n);
		FOR(i, 0, d)
			solutions.pb(mod(x + i*(n/d), n));
	}
	return solutions;
}

```
Used to solve linear equation system in modular aritmetic. Finds all solutions to _ax = b (mod n)_. Relies on Euclid's extended algorithm.

### Modular Inverse

```cpp

int mod_inverse(int a, int n) {
	int x, y;
	int d = extended_euclid(a, n, x, y);
	if (d > 1) return -1;
	return mod(x,n);
}

```
In modular arithmetic we do not have a division operation. However, we do have modular inverses.

1) The modular inverse of A (mod C) is A^-1
2) (A * A^-1) ≡ 1 (mod C) or equivalently (A * A^-1) mod C = 1
3) Only the numbers coprime to C (numbers that share no prime factors with C) have a modular inverse (mod C)

This is the fastest method for finding the inverse of A (mod C) using Euclid's extended algorithm.

Computes _b_ such that _ab = 1 (mod n)_, returns -1 on failure.

### Chinese Remainder Theorem

```cpp

int chinese_remainder(int *n, int *a, int len) {
	int p, i, prod = 1, sum = 0;
	for (i = 0; i < len; i++) prod *= n[i];
	for (i = 0; i < len; i++) {
		p = prod / n[i];
		sum += a[i] * mod_inverse(p, n[i]) * p;
	}
	return sum % prod;
}

```
The Chinese remainder theorem is a theorem of number theory, which states that if one knows the remainders of the Euclidean division of an integer _n_ by several integers, then one can determine uniquely the remainder of the division of _n_ by the product of these integers, under the condition that the divisors are pairwise coprime. This theorem is widely used for computing with large integers, as it allows replacing a computation for which one knows a bound on the size of the result by several similar computations on small integers.

_n_: _len_ numbers which are pairwise coprime.

_a_: remainders of these _n_ numbers when an unknown number _x_ is divided by them. 

_len_ is the length of _n_ and _a_.

Returns _x = a[i] (mod n[i])_.
