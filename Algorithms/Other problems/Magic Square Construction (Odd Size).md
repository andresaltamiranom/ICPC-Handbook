# Magic Square Construction (Odd Size)

## Problem Description

A magic square is a 2D array of size _n_ × _n_ that contains integers from [1..n^2] with ‘magic’ property: The sum of integers in each row, column, and diagonal is the same. For example, for _n_ = 5, we can have the following magic square below that has row sums, column sums, and diagonal sums equals to 65.

![magic square](https://i.imgur.com/GuIeqZ9.png)

Our task is to construct a magic square given its size _n_, assuming that _n_ is odd.

## Solution(s)

If we do not know the solution, we may have to use the standard recursive backtracking routine that tries to place each integer ∈ [1..n^2] one by one. Such Complete Search solution is too slow for a large _n_.

Fortunately, there is a nice ‘construction strategy’ for magic square of odd size called the ‘Siamese (De la Loubère) method’. We start from an empty 2D square array. Initially, we put integer 1 in the middle of the first row. Then we move northeast, wrapping around as necessary. If the new cell is currently empty, we add the next integer in that cell. If the cell has been occupied, we move one row down and continue going northeast. This Siamese method is shown in the figure below. We reckon that deriving this strategy without prior exposure to this problem is likely not straightforward (although not impossible if one stares at the structure of several odd-sized Magic Squares long enough).

![magic square](https://i.imgur.com/BFbTQk8.png)

There are other special cases for Magic Square construction of different sizes. It may be unnecessary to learn all of them as most likely it will not appear in a programming contest. However, we can imagine some contestants who know such Magic Square construction strategies will have an advantage in case such problem appears.
