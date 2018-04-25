# Josephus Problem

## Problem Description

The Josephus problem is a classic problem where initially there are _n_ people numbered from 1, 2, ..., _n_, standing in a circle. Every _k_-th person is going to be executed and removed from the circle. This count-then-execute process is repeated until there is only one person left and this person will be saved (history said that he was the person named Josephus).

## Solution(s)

### Complete Search for Smaller Instances

The smaller instances of Josephus problem are solvable with Complete Search by simply simulating the process with the help of a cyclic array or a circular linked list. The larger instances of Josephus problem require better solutions.

### Special Case when _k_ = 2

There is an elegant way to determine the position of the last surviving person for _k_ = 2 using the binary representation of the number _n_. If n = 1\*b1b2b3..bn then the answer is b1b2b3..bn*1, i.e. we move the most significant bit of _n_ to the back to make it the least significant bit. This way, the Josephus problem with _k_ = 2 can be solved in O(1).

### General Case

Let _F(n, k)_ denote the position of the survivor for a circle of size _n_ and with _k_ skipping rule and we number the people from 0, 1, ..., _n_ − 1 (we will later add +1 to the final answer to match the format of the original problem description above). After the _k_-th person is killed, the circle shrinks by one to size _n_ − 1 and the position of the survivor is now _F(n−1, k)_. This relation is captured with equation _F(n, k) = (F (n − 1, k) + k) % n_. The base case is when _n_ = 1 where we have _F(1, k) = 0_. This recurrence has a time complexity of O(n).
