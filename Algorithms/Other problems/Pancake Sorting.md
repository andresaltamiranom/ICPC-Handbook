# Pancake Sorting

## Problem Description

Pancake Sorting is a classic Computer Science problem, but it is rarely used. This problem can be described as follows: You are given a stack of N pancakes. The pancake at the bottom and at the top of the stack has **index 0** and **index N-1**, respectively. The size of a pancake is given by the pancake's diameter (an integer ∈ [1 .. MAX_D]), where MAX_D is the maximum diameter for a pancake. All pancakes in the stack have **different** diameters. For example, a stack A of N = 5 pancakes: {3, 8, 7, 6, 10} can be visualized as:

![Pancake Sorting](https://i.imgur.com/DVzjSQt.png)

Your task is to sort the stack in **descending order** - that is, the largest pancake is at the bottom and the smallest pancake is at the top. However, to make the problem more real-life like, sorting a stack of pancakes can only be done by a sequence of pancake 'flips', denoted by function _flip(i)_. A _flip(i)_ move consists of inserting a spatula between two pancakes in a stack (at **index i** and **index N-1**) and flipping (reversing) the pancakes on the spatula (reversing the sub-stack [**i .. N-1**]).

For example, stack A can be transformed to stack B via _flip(0)_, i.e. inserting a spatula between index 0 and 4 then flipping the pancakes in between. Stack B can be transformed to stack C via _flip(3)_. Stack C can be transformed to stack D via _flip(1)_. And so on... Our target is to make the stack sorted in **descending order**, i.e. we want the final stack to be like stack E.
	
![Pancake Sorting](https://i.imgur.com/dnTnjj1.png)

To make the task more challenging, you have to compute the **minimum number of _flip(i)_ operations** that you need so that the stack of N pancakes is sorted in descending order.

You are given an integer T in the first line, and then T test cases, one in each line. Each test case starts with an integer N, followed by N integers that describe the initial content of the stack. You have to output one integer, the minimum number of _flip(i)_ operations to sort the stack.

Constraints: 1 ≤ T ≤ 100, 1 ≤ N ≤ 10, and N ≤ MAX_D ≤ 1000000.
	
## Solution(s)

First, we need to make an observation that the diameters of the pancake do not really matter. We just need to write simple code to sort these (potentially huge) pancake diameters from [**1..1 million**] and relabel them to [**0..N-1**]. This way, we can describe any stack of pancakes as simply a permutation of N integers.

If we just need to get the pancakes sorted, we can use a non optimal O(2 * N - 3) Greedy algorithm: Flip the largest pancake to the top, then flip it to the bottom. Flip the second largest pancake to the top, then flip it to the second from the bottom. And so on. If we keep doing this, we will be able to have a sorted pancake in O(2 * N - 3) steps, regardless of the initial state.

However, to get the minimum number of flip operations, we need to be able to model this problem as a Shortest Paths problem on unweighted State-Space graph. The vertex of this State-Space graph is a permutation of N pancakes. A vertex is connected with unweighted edges to O(N - 1) other vertices via various flip operations (minus one, as flipping the topmost pancake does not change anything). We can then use BFS from the starting permutation to find the shortest path to the target permutation (where the permutation is sorted in descending order). There are up to V = O(N!) vertices and up to V = O(N! * (N - 1)) in this State-Space graph. Therefore, an O(V + E) BFS runs in O(N * N!) per test case or O(T * N * N!) for all test cases. Note that coding such BFS is already a challenging task. But this solution is still too slow for the largest case.

A simple optimization is to run BFS from the target permutation (sorted descending) to all other permutations **only once**, for all possible N in [**1..10**]. This solution has time complexity of roughly O(10 * N * N! + T), much faster than before but still too slow for typical programming contest settings.

A better solution is a more sophisticated search technique called 'meet in the middle' (bidirectional BFS) to bring down the search space to a manageable level. First, we do some preliminary analysis to identify that for the largest test case when N = 10, we need _at most_ 11 flips to sort any input stack to the sorted one. Therefore, we precalculate BFS from the target permutation to all other permutations for all N ∈ [**1..10**], but stopping as soon as we reach depth _floor(11/2)_ = 5. Then, for each test case, we run BFS from the starting permutation again with maximum depth 5. If we encounter a common vertex with the precalculated BFS from target permutation, we know that the answer is the distance from starting permutation to this vertex plus the distance from target permutation to this vertex. If we do not encounter a common vertex at all, we know that the answer should be the maximum flips: 11. On the largest test case with N = 10 for all test cases, this solution has time complexity of roughly O((10 + T) * 10^5), which is now feasible.
