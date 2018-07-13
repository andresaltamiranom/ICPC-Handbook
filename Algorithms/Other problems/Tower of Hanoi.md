# Tower of Hanoi

## Problem Description

The classic description of the problem is as follows: There are three pegs: A, B, and C, as well as n discs, with all discs having different sizes. Starting with all the discs stacked in ascending order on one peg (peg A), your task is to move all n discs to another peg (peg C). No disc may be placed on top of a disc smaller than itself, and only one disc can be moved
at a time, from the top of one peg to another.

## Solution(s)

There exists a simple recursive backtracking solution for the classic Tower of Hanoi problem. The problem of moving n discs from peg A to peg C with additional peg B as intermediate peg can be broken up into the following sub-problems:

1. Move n − 1 discs from peg A to peg B using peg C as the intermediate peg. After this recursive step is done, we are left with disc n by itself in peg A.

2. Move disc n from peg A to peg C.

3. Move n − 1 discs from peg B to peg C using peg A as the intermediate peg. These n − 1 discs will be on top of disc n which is now at the bottom of peg C.

Note that step 1 and step 3 are recursive steps. The base case is when n = 1 where we simply move a single disc from the current source peg to its destination peg, bypassing the intermediate peg. A sample C++ implementation code is shown below:

```cpp

#include <cstdio>
using namespace std;

void solve(int count, char source, char destination, char intermediate) {
	if (count == 1)
		printf("Move top disc from pole %c to pole %c\n", source, destination);
	else {
		solve(count-1, source, intermediate, destination);
		solve(1, source, destination, intermediate);
		solve(count-1, intermediate, destination, source);
	}
}

int main() {
	solve(3, ’A’, ’C’, ’B’); // try larger value for the first parameter
} // return 0;

```

The minimum number of moves required to solve a classic Tower of Hanoi puzzle of n discs using this recursive backtracking solution is 2^n − 1 moves.
