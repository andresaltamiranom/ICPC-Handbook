# Matrices

A matrix is a mathematical concept that corresponds to a two-dimensional array in programming.
Following are some of the most common operations that can appear in competitive programming regarding matrices.

### Definitions

```cpp
typedef vector<vector<double> > Matrix;
#define EPS 1E-7
#define CREATE(R, C) Matrix(R, vector<double>(C));
```

### Identity

```cpp
Matrix identity(int n) {
	Matrix m = CREATE(n, n);
	FOR(i, 0, n)
		m[i][i] = 1;
	return m;
}
```

**Input:** An integer representing the size of the identity matrix.

**Output:** A _n_ × _n_ identity matrix.

The identity matrix is a square matrix in which all the elements of the principal diagonal are 1 and all other elements are 0. The effect of multiplying a given matrix by an identity matrix is to leave the given matrix unchanged.

### Constant multiplication

```cpp
Matrix multiply(Matrix m, double k) {
	FOR(i, 0, m.size())
		FOR(j, 0, m[0].size())
			m[i][j] *= k;
	return m;
}
```

**Input:** A matrix _m_ and a double _k_.

**Output:** The matrix _m_ multiplied by the value _k_.

Multiplies each value of _m_ by _k_ and returns the resulting matrix.

### Matrix Multiplication

```cpp
Matrix multiply(Matrix m1, Matrix m2) {
	Matrix result = CREATE(m1.size(), m2[0].size());
	if(m1[0].size() != m2.size())
		return result;
	FOR(i, 0, result.size())
		FOR(j, 0, result[0].size())
			FOR(k, 0, m1[0].size())
				result[i][j] += m1[i][k]*m2[k][j];
	return result;
}
```

**Input:** Two matrices, _m1_ and _m2_.

**Output:** The resulting multiplied matrix.

The product AB of matrices A and B is defined if A is of size _a_ × _n_ and B is of size _n_ × _b_, i.e., the width of A equals the height of B. The result is a matrix of size _a_ × _b_. Matrix multiplication is associative, so A(BC) = (AB)C holds, but it is not commutative, so AB = BA does not usually hold. Using a straightforward algorithm, we can calculate the product of two _n_ × _n_ matrices in O(n^3) time. There are also more efficient algorithms for matrix multiplication, but they are mostly of theoretical interest and such algorithms are not necessary in competitive programming.

###  Matrix Power

```cpp
Matrix pow(Matrix m, int exp) {
	if(!exp) return identity(m.size());
	if(exp == 1) return m;
	Matrix result = identity(m.size());
	while(exp) {
		if(exp & 1) result = multiply(result, m);
		m = multiply(m, m);
		exp >>= 1;
	}
	return result;
}
```

**Input:** A matrix _m_ and an integer _exp_ by which the matrix's power will be raised to.

**Output:** The matrix _m_ raised to the power _exp_.

The power A^k of a matrix A is defined only if A is a square matrix. The matrix A^k is calculated in O((n^3)log(k)) time.

### Gauss Jordan

```cpp
double gaussJordan(Matrix &a, Matrix &b) {
	int n = a.size(), m = b[0].size();
	vi irow(n), icol(n), ipiv(n);
	double det = 1;
	FOR(i, 0, n) {
		int pj = -1, pk = -1;
		FOR(j, 0, n) if (!ipiv[j])
			FOR(k, 0, n) if (!ipiv[k])
				if (pj == -1 || abs(a[j][k]) > abs(a[pj][pk])) { pj = j; pk = k; }
		if (abs(a[pj][pk]) < EPS) { cerr << "Matrix is singular." << endl; exit(0); }
		ipiv[pk]++;
		swap(a[pj], a[pk]);
		swap(b[pj], b[pk]);
		if (pj != pk) det *= -1;
		irow[i] = pj;
		icol[i] = pk;

		double c = 1.0 / a[pk][pk];
		det *= a[pk][pk];
		a[pk][pk] = 1.0;
		FOR(p, 0, n) a[pk][p] *= c;
		FOR(p, 0, m) b[pk][p] *= c;
		FOR(p, 0, n) if (p != pk) {
			c = a[p][pk];
			a[p][pk] = 0;
			FOR(q, 0, n) a[p][q] -= a[pk][q] * c;
			FOR(q, 0, m) b[p][q] -= b[pk][q] * c;      
		}
	}
	for(int p = n-1; p >= 0; p--) if (irow[p] != icol[p]) {
		FOR(k, 0, n) swap(a[k][irow[p]], a[k][icol[p]]);
	}
	return det;
}
```

**Input:** Two matrices _a_ and _b_ passed by reference.

**Output:** Solves AX=B. Returns A^-1 in A, X in B, and returns det(A).

A method of solving a linear system of equations. This is done by transforming the system's augmented matrix into row-reduced echelon form by means of row operations.

### rref

```cpp
int rref(Matrix &a) {
	int n = a.size(), m = a[0].size();
	int r = 0;
	FOR(c, 0, m) {
		int j = r;
		FOR(i, r+1, n)
			if (abs(a[i][c]) > abs(a[j][c])) j = i;
		if (abs(a[j][c]) < EPS) continue;
		swap(a[j], a[r]);
		double s = 1.0 / a[r][c];
		FOR(j, 0, m) a[r][j] *= s;
		FOR(i, 0, n) if (i != r) {
			double t = a[i][c];
			FOR(j, 0, m) a[i][j] -= t * a[r][j];
		}
		r++;
	}
	return r;
}
```

**Input:** A matrix _a_ passed by reference.

**Output:** Converts the matrix _a_ into its row-reduced echelon form and returns the rank of _a_.

A matrix is in reduced-row echelon form if it satisfies the following conditions:

1) It is in row echelon form.

2) Every leading coefficient is 1 and is the only nonzero entry in its column.

Unlike the row echelon form, the reduced-row echelon form of a matrix is unique and does not depend on the algorithm used to compute it. For a given matrix, despite the row echelon form not being unique, all row echelon forms and the reduced-row echelon form have the same number of zero rows and the pivots are located in the same indices.
