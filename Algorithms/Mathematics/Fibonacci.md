# Fibonacci

### Fibonacci using golden ratio

```cpp

int fibn(int n) {
	double goldenRatio = (1+sqrt(5))/2;
	return round((pow(goldenRatio, n+1) - pow(1-goldenRatio, n+1))/sqrt(5));
}

```

**Input:** An integer number between 0 and 91.

**Output:** The _Nth_ fibonacci number.

This function gets the _Nth_ fibonacci number in approximately O(1) time. However, this function is not too accurate for large Fibonacci numbers, allowing us to use it only for numbers up to the 91th Fibonacci number without a mistake.

### Fibonacci

```cpp

int fibonacci(int n) {
	Matrix m = CREATE(2, 2);
	m[0][0] = 1, m[0][1] = 1, m[1][0] = 1, m[1][1] = 0;
	Matrix fib0 = CREATE(2, 1);
	fib0[0][0] = 1, fib0[1][0] = 1;
	Matrix r = multiply(pow(m, n), fib0);
	return r[1][0];
}

```

**Input:** An integer number.

**Output:** The _Nth_ fibonacci number.

Calculates the _Nth_ fibonacci number. This function uses the efficient matrix power to perform in O(log(n)) time. In this solution, we create a matrix of the form [[1,1],[1,0]] and multiply it by itself _N_ times (raise it to the power of _N_). The resulting matrix will contain the _Nth_ fibonacci number at M[0,1] and M[1,0], so just return either.

Use this function if you are looking to calculate a **single** fibonacci number. If you are looking for all fibonacci numbers up to _N_, use a O(n) DP solution instead.
