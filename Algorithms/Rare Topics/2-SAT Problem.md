# 2-SAT Problem

## Problem Description

You are given a conjunction of disjunctions (“and of ors”) where each disjunction (“the or operation”) has two arguments that may be variables or the negation of variables. The disjunctions of pairs are called as ‘clauses’ and the formula is known as the 2-CNF (Con- junctive Normal Form) formula. The 2-SAT problem is to find a truth (that is, true or false) assignment to these variables that makes the 2-CNF formula true, i.e. every clause has at least one term that is evaluated to true.

**Example 1**: (x1 ∨ x2) ∧ (¬x1 ∨ ¬x2) is satisfiable because we can assign x1 = true and x2 = false (alternative assignment is x1 = false and x2 = true).

**Example 2**: (x1 ∨ x2) ∧ (¬x1 ∨ x2) ∧ (¬x2 ∨ x3) ∧ (¬x2 ∨ ¬x3) is not satisfiable. You can try all 8 possible combinations of boolean values of x1, x2, and x3 to realize that none of them can make the 2-CNF formula satisfiable.

## Solution(s)

### Complete Search

Contestants who only have a vague knowledge of the Satisfiability problem may have thought that this problem is an NP-Complete problem and therefore attempt a complete search solution. If the 2-CNF formula has _n_ variables and _m_ clauses, trying all 2^n possible assignments and checking each assignment in O(m) has an overall time complexity of O(2^n × m). This is likely TLE.

The 2-SAT is a special case of Satisfiability problem and it admits a polynomial solution like the one shown below.

### Reduction to Implication Graph and Finding SCC

First, we have to realize that a clause in a 2-CNF formula (a ∨ b) ≡ (¬a ⇒ b) ≡ (¬b ⇒ a). Thus, given a 2-CNF formula, we can build the corresponding ‘implication graph’. Each variable has two vertices in the implication graph, the variable itself and the negation/inverse of that variable. An edge connects one vertex to another if the corresponding variables are related by an implication in the corresponding 2-CNF formula. For the two 2-CNF example formulas above, we have the following implication graphs:

![2-SAT](https://i.imgur.com/uKqVr1k.png)

As you can see in the example, a 2-CNF formula with _n_ variables (excluding the negation) and _m_ clauses will have V = θ(2n) = θ(n) vertices and E = O(2m) = O(m) edges in the implication graph.
Now, a 2-CNF formula is satisfiable if and only if “there is no variable that belongs to the same Strongly Connected Component (SCC) as its negation”.

In the left figure of the example, we see that there are two SCCs: {0,3} and {1,2}. As there is no variable that belongs to the same SCC as its negation, we conclude that the 2-CNF formula shown in the left example is satisfiable.

In the right figure of the example, we observe that all six vertices belong to a single SCC. Therefore, we have vertex 0 (that represents x1) and vertex 1 (that represents ¬x1), vertex 2 (x2) and vertex 3 (¬x3), vertex 4 (x3) and vertex 5 (¬x3) in the same SCC. Therefore, we conclude that the 2-CNF formula shown in the right example is not satisfiable.

To find the SCCs of a directed graph, we can use either Tarjan’s SCC algorithm or Kosaraju’s SCC algorithm.
