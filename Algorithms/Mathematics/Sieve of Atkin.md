# Sieve of Atkin

The sieve of Atkin is a modern algorithm for finding all prime numbers up to a specified integer. Compared with the sieve of Eratosthenes, which marks off multiples of primes, the sieve of Atkin does some preliminary work and then marks off multiples of squares of primes, thus achieving a better theoretical asymptotic complexity.

This algorithm can compute primes up to N using only O(N/log(log(N))) operations with only O(n/log(log(N))) bits of memory. This is a slightly better performance than the sieve of Eratosthenes which uses O(N*log(log(N))) operations.

**How it works:**

1. The algorithm treats 2, 3 and 5 as special cases and just adds them to the set of primes to start with.

2. Like Sieve of Eratosthenes, we start with a list of numbers we want to investigate. Suppose we want to find primes <= 100, then we make a list for [5, 100]. Remember that 2, 3 and 5 are special cases and 4 is not a prime.

3. The algorithm talks in terms of modulo-60 remainders.

4. All numbers with modulo-sixty remainder 1, 13, 17, 29, 37, 41, 49, or 53 have a modulo-twelve remainder of 1 or 5. These numbers are prime if and only if the number of solutions to 4x^2 + y^2 = n is odd and the number is squarefree. (A square free integer is one which is not divisible by any perfect square other than 1.)

5. All numbers with modulo-sixty remainder 7, 19, 31, or 43 have a modulo-twelve remainder of 7. These numbers are prime if and only if the number of solutions to 3x^2 + y^2 = n is odd and the number is squarefree.

6. All numbers with modulo-sixty remainder 11, 23, 47, or 59 have a modulo-twelve remainder of 11. These numbers are prime if and only if the number of solutions to 3x^2 – y^2 = n is odd and the number is squarefree.

The following code stores in a boolean vector _isPrime_ whether the numbers in the range 1..N are prime or not, and then generates a vector _primes_ of prime numbers within that range.

```cpp

typedef vector<ll> vll;
vll primes;
void sieve_atkins(ll n) {
   vector<bool> isPrime(n + 1);
   isPrime[2] = isPrime[3] = isPrime[5] = true;
   for (ll i = 6; i <= n; i++)
      isPrime[i] = false;

   ll lim = ceil(sqrt(n));
   for (ll x = 1; x <= lim; x++) {
      for (ll y = 1; y <= lim; y++) {
         ll num = (4 * x * x + y * y);
         if (num <= n && (num % 12 == 1 || num % 12 == 5))
            isPrime[num] = true;
         num = (3 * x * x + y * y);
         if (num <= n && (num % 12 == 7))
            isPrime[num] = true;
         if (x > y) {
            num = (3 * x * x - y * y);
            if (num <= n && (num % 12 == 11))
               isPrime[num] = true;
         }
      }
   }

   for (ll i = 5; i <= lim; i++)
      if (isPrime[i])
         for (ll j = i * i; j <= n; j += i)
            isPrime[j] = false;

   for (ll i = 2; i <= n; i++)
      if (isPrime[i])
         primes.pb(i);
}

```
