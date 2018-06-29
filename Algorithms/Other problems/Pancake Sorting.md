# Pancake Sorting

## Problem Description

Pancake Sorting is a classic Computer Science problem, but it is rarely used. This problem can be described as follows: You are given a stack of N pancakes. The pancake at the bottom and at the top of the stack has **index 0** and **index N-1**, respectively. The size of a pancake is given by the pancake's diameter (an integer ∈ [1 .. MAX_D]). All pancakes in the stack have **different** diameters. For example, a stack A of N = 5 pancakes: {3, 8, 7, 6, 10} can be visualized as:

// IMAGE

Your task is to sort the stack in **descending order** - that is, the largest pancake is at the bottom and the smalles pancake is at the top. However, to make the problem more real-life like, sorting a stack of pancakes can only be done by a sequence of pancake 'flips', denoted by function _flip(i)_. A _flip(i)_ move consists of inserting a spatula between two pancakes in a stack (at **index i** and **index N-1**) and flipping (reversing) the pancakes on the spatula (reversing the sub-stack [i .. N-1]).

For example, stack A can be transformed to stack B via _flip(0), i.e. inserting a spatula between index 0 and 4 then flipping the pancakes in between. Stack B can be transformed to stack C via _flip(3)_. Stack C can be transformed to stack D via _flip(1)_. And so on... Our target is to make the stack sorted in **descending order**, i.e. we want the final stack to be like stack E.
	
// IMAGE

To make the task more challenging, you have to compute the **minimum number of _flip(i)_ operations** that you need so that the stack of N pancakes is sorted in descending order.
	
## Solution(s)

// TODO
