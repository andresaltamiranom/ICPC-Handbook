# Bracket Matching

## Problem Description

Programmers are very familiar with various form of braces: ‘()’, ‘{}’, ‘[]’, etc. as they use braces quite often in their code, especially when dealing with _if_ statements and loops. Braces can be nested, e.g. ‘(())’, ‘{{}}’, ‘[[]]’, etc. A well-formed code must have a matched set of braces. The Bracket Matching problem usually involves a question on whether a given set of braces is properly nested. For example, ‘(())’, ‘({})’, ‘(){}[]’ are correctly matched braces whereas ‘(()’, ‘(}’, ‘)(’ are **not** correct.

## Solution(s)

We read the brackets one by one from left to right. Every time we encounter a close bracket, we need to match it with the latest open bracket (of the same type). This matched pair is then removed from consideration and the process is continued. This requires a ‘Last In First Out’ data structure: Stack.

We start from an empty stack. Whenever we encounter an open bracket, we push it into the stack. Whenever we encounter a close bracket, we check if it is of the same type with the top of the stack. This is because the top of the stack is the one that has to be matched with the current close bracket. Once we have a match, we pop the topmost bracket from the stack to remove it from future consideration. Only if we manage to reach the last bracket and find that the stack is back to empty, then we know that the brackets are properly nested.

As we examine each of the _n_ braces only once and all stack operations are O(1), this algorithm clearly runs in O(n).

## Variant(s)

The number of ways _n_ pairs of parentheses can be correctly matched can be found with the Catalan formula. The optimal way to multiply matrices (i.e. the Matrix Chain Multiplication problem) also involves bracketing. This variant can be solved with Dynamic Programming.
