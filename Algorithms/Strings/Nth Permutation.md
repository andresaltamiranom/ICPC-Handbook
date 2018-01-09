# Nth Permutation

```cpp

string nthPermutation(string seq, int permNum) {
	if(!seq.length()) return "";
	int f = fact(seq.length() - 1);
	int q = permNum/f, r = permNum%f;
	return seq[q] + nthPermutation(seq.substr(0, q) + seq.substr(q+1), r);
}

```

**Input:** A string to find the permutation of and an integer of the number of permutation to find. Note: _seq_ must be sorted.

**Output:** The _Nth_ permutation of _seq_.

Gets the _Nth_ permutation of a sorted string using the Factoradic number system.

To get the factoradic representation of a number, we follow the following rule.
The digit at the _Nth_ place can take any value between _0_ and _n_.

So the first few numbers represented as factoradics are:

0 -> 0 = 0\*0!
1 -> 10 = 1\*1! + 0\*0!
2 -> 100 = 1\*2! + 0\*1! + 0\*0!
3 -> 110 = 1\*2! + 1\*1! + 0\*0!
4 -> 200 = 2\*2! + 0\*1! + 0\*0!
5 -> 210 = 2\*2! + 1\*1! + 0\*0!
6 -> 1000 = 1\*3! + 0\*2! + 0\*1! + 0\*0!
7 -> 1010 = 1\*3! + 0\*2! + 1\*1! + 0\*0!
8 -> 1100 = 1\*3! + 1\*2! + 0\*1! + 0\*0!
9 -> 1110 = 1\*3! + 1\*2! + 1\*1! + 0\*0!
10-> 1200 = 1\*3! + 2\*2! + 0\*1! + 0\*0!
.
.
and so on.

There is a direct relationship between the _Nth_ lexicographical permutation of a string and its factoradic representation.

Once we represent _n_ in factoradic representation, we consider each digit in it and add a character from the given string to the output. For example, if we need to find the 14-th permutation of ‘abcd’, the factoradic representation of 14 is 2100. So we start and take the letter at index 2, which is _c_. From the resulting string 'abd', we take the letter at index 1, which is _b_. Then at index 0, which is _a_, and then again at index 0, which is _d_. This results in the string 'cbad', the 14th permutation of 'abcd'.

This function works the same by finding the factorial given the length of the string, and storing in _q_ the letter at the index of the first value of the factoradic number, and calling the same function recursively, removing from the string the letters that have been already selected.
