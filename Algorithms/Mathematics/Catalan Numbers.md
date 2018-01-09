# Catalan Numbers

Catalan numbers are a sequence of positive integers that appear in many counting problems in combinatorics.

Examples of problems that can be solved using catalan numbers are:

	- Counting the number of expressions containing _n_ pairs of parentheses which are correctly matched.
	- Counting the number of different ways _n + 1_ factors can be completely parenthesized.
	- Counting the number of full binary trees with _n + 1_ leaves.
	- Counting the number of different ways a convex polygon with _n + 2_ sides can be cut into triangles by connecting vertices with non-crossing line segments.
 
## Helper Functions

### Factorial

```cpp

int fact(int n) {
    return n ? n*fact(n-1) : 1;
}

```

**Input:** An integer.

**Output:** The factorial of the given integer _n_.

Calculates the factorial of a given number.

## Catalan Numbers Functions

### nthCatalan

```cpp

int nthCatalan(int n) {
	return fact(2*n)/(pow(fact(n), 2)*(n+1));
}

```

**Input:** An integer.

**Output:** The Nth catalan number.

The Nth Catalan number is given directly in terms of binomial coefficients by the following closed formula:

![alt text](https://wikimedia.org/api/rest_v1/media/math/render/svg/34d4f28865115a05a806649a40f84e1bbc736320)

### nextCatalan

```cpp

int nextCatalan(int n, int previous) {
	return previous*2*(2*n+1)/(n+2);
}

```

**Input:** The index of the Catalan number we want to obtain (_N+1_), and the value of the Catalan number at index _n-1_. 

**Output:** The (N+1)th Catalan number.

All Catalan numbers follow satisfy the following recurrence relations:

![alt text](https://wikimedia.org/api/rest_v1/media/math/render/svg/79756c79f56fb6c5669408007a9152217e610580)

To get the Nth+1 Catalan number, we just plug in the given formula.
