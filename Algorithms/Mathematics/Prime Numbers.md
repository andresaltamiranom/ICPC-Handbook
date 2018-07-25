# Prime Numbers

A number _a_ is a factor (or divisor) of _b_ if _a_ divides _b_ without leaving any residue. For example, the factors of 32 are 1, 2, 4, 8, 16, and 32. A number _n_ (greater than one) is prime if its only divisors are 1 and _n_. For example, the number 7 is prime because its only divisors are 1 and 7.

The following functions are useful when working with problems that involve prime numbers and factors.

### Global Variables

```cpp

#define SIZE 1000000
bitset<SIZE> sieve;
vi primesList;
```

### buildSieve

```cpp

void buildSieve() {
	sieve.set();
	sieve[0] = sieve[1] = 0;
	int root = sqrt(SIZE);
	FOR(i, 2, root+1)
		if (sieve[i])
			for(int j = i*i; j < SIZE; j+=i)
				sieve[j] = 0;
}
```
**Input:** None, but it uses the bitset _sieve_ declared globally.

**Output:** Does not return any value.

Builds a sieve that stored in each index a 1 if the index's value is prime and a 0 if it is not. First establishes the bitset with all of its bits in 1, except for indices 0 and 1. Then iterates from 2 to the square root of _SIZE_ + 1. If the current index is a 1, it sets every index that is multiple of _i_ to 0, starting from the square of _i_. This repeats until reaching the root. When it reaches the root, only the prime indices would be the ones not accessed and therefore left with a value of 1.


### buildPrimesList

```cpp

void buildPrimesList() {
	if(!sieve[2])
		buildSieve();
	primesList.reserve(SIZE/log(SIZE));
	FOR(i, 2, SIZE+1)
		if(sieve[i])
			primesList.pb(i);
}
```
**Input:** None, but it inserts the prime numbers generated into the _primesList_ vector declared globally.

**Output:** None.

Using the sieve constructed in the buildSieve() functions, gets all the prime numbers from 2 to SIZE + 1. If the sieve has not yet been constructed, it calls the buildSieve() function. It then reserves the space that the vector will use. Finally, iterates from 2 to SIZE + 1. If the iterator _i_ is found in the sieve with a value of 1, it adds it to _primesList_.


### primeFactorization

```cpp

vii primeFactorization(int N) {
	vii factors;
	int idx = 0, pf = primesList[0];
	while(pf*pf <= N) {
		while(N%pf==0) {
			N /= pf;
			if(factors.size() && factors.back().first == pf)
				factors.back().second++;
			else
				factors.pb(ii(pf, 1));
		}
		pf = primesList[++idx];
 	}
	if(N!=1) factors.pb(ii(N, 1));
	return factors;
}
```
**Input:** An integer number whose prime factorization we want to obtain.

**Output:** A vector of pair of integers. Each pair has the following representation: the first element of the pair represents the prime number within the prime factorization, while the second element of the pair represents the amount of times that the first element is repeated in the prime factorization. For example: The prime factorization of 540 is 2^2 * 3^3 * 5^1. Therefore, this function would return a vector of pair of integers like the following: < (2,2), (3,3), (5,1) >.

Prior to calling this function, one must have already the list of prime numbers created by the function _buildPrimesList_. Once this list is filled, we start with the first prime number. We will iterate until the square root of N and, within this loop, another loop will divide _N_ by the current prime factor. While _N_ can be divided, the amount of times that the current prime number appears in the prime factorization will be incremented by 1. If this is the first time that this prime number divides _N_, the prime number is added to _factors_ with a value of 1. If it is not the first time, we add 1 to the second element of the current pair (factors.back()). When the current prime number can no longer divide _N_, we get the next prime number from _primesList_ and repeat this process until the current prime number _pf_ is greater than the square root of _N_. When this process is finished, we check if _N_ is different from 1. If it is different, it means that _N_, after being divided, is the last prime factor, so we add it to the list.


### getDivisors

```cpp

void getDivisors(vii pf, int d, int index, vi &div) {
	if (index == pf.size()) {
		div.pb(d);
		return;
	}
	for (int i = 0; i <= pf[index].second; i++) {
		getDivisors(pf, d, index+1, div);
		d *= pf[index].first;
	}
	return;
}
```
**Input:** A vector of pair of integers that represent a prime factorization, an integer representing the current divisor, an integer representing the index in which we start to iterate over the prime factors, and a vector of integers passed by reference which will store the divisors.

**Output:** None, but all the divisors are inserted into _div_, which was passed by reference.

This is a recursive function. If the index received is equal to the size of the vector of prime factors (where the size of this vector represents the amount of distinct prime numbers found in the vector), we insert the current value _d_ into _div_. If not, we iterate with a _for_ loop the amount of times that the current prime factor is found in the prime factorization. For each of these, we call _getDivisors()_ recursively with the same parameters, but incrementing the current index by 1 and then multiplying the current divisor by the current prime factor. When this loops repeats, it will call itself recursively again, but now the divisor to insert will have been multiplied by a prime number from the prime factorization. This will go on until all prime factors have been multiplied and every combination of the product of the prime numbers in the prime factorization has beedn added to _div_, thus generating all possible divisors.


### divisors

```cpp

vi divisors(ll N) {
	vii pf = primeFactorization(N);
	vi div;
	getDivisors(pf, 1ll, 0, div);
	sort(div.begin(), div.end());
	return div;
}
```
**Input:** An integer whose divisors we want to obtain.

**Output:** A vector of integers, divisors of _N_.

Given a number _N_, returns all the divisors of _N_. To do this, we first obtain the prime factorization of N through the function _primeFactorization()_. Then, we get all the divisors of _N_ by calling _getDivisors()_. Finally, we sort the divisors to return a list of divisors ordered from least to greatest (this step can be skipped if the order of the divisors in the vector does not matter).


### IsPrime

```cpp

bool isPrime(int n) {
	if(n < 2) return false;
	if(n == 2 || n == 3) return true;
	if(!(n&1 && n%3)) return false;
	long long sqrtN = sqrt(n)+1;
	for(long long i = 6LL; i <= sqrtN; i += 6)
		if(!(n%(i-1)) || !(n%(i+1))) return false;
	return true;
}

```

**Input:** An integer number.

**Output:** A boolean value.

- If a number is less than 2, it is not prime.

- If a number is 2 or 3, it is prime.

- If a number is divisible by 2 or 3 and this nunmber is greater than 3, it is not prime.

Onwards from that we iterate until the square root of the number. If a number _n_ is not prime, it can be represented as a product of _a_ · _b_, where _a_ ≤ √n or _b_ ≤ √n, therefore, it is safe to assume that at least one of the factors is within the range 2 to √n. Knowing that at least one factor is ≤ √n, we can iterate only until the square root of _n_, checking if _n_ % _i_ (_n_ modulo _i_) returns 0 as the remainder. If we reach the square root and no number divided _n_ without leaving a remainder, then we know that _n_ is prime. Starting in 5 and incrementing by 6 each loop, we can skip all multiples of 2 and 3. This allows us to check only two thirds of all the numbers withing the range 2 to √n. Within the _if_ condition, we have to check if _n_ is divisible by _i_ - 1 and _i_ + 1, as these are the only two values in our increment by 6 that we have not yet checked. This function runs on O(√n) time.
