# Knuth–Morris–Pratt String Matching

String Matching is a problem of finding the starting index (or indices) of a (sub)string (called _pattern_ P) in a longer string (called _text_ T).

**Example:**

Assume that we have T = "STEVEN EVENT".
- If P = "EVE", then the answers are indices 2 and 7.
- If P = "EVENT", then the answer is index 7 only.
- If P = "EVENING", then there is no answer.

### Naïve solution

A naïve solution to this problem can run in O(n) **on average**, but it can run in O(n*m) with the worst case input, like T = "AAAAAAAAAAB" and P = "AAAAB". The naïve algorithm will keep failing at the last character of pattern P and then try the next starting index, which is just +1 than the previous attempt. This is not efficient.

### KMP solution

KMP uses the information gained by previous character comparisons, especially those that match. KMP _never_ re-compares a character in T that has matched a character in P.

// TODO

```cpp

/* KMP
O(N+M)
Searches for a pattern in a string
*/
vi buildTable(string& pattern) {
	vi table(pattern.length()+1);
	int i = 0, j = -1, m = pattern.length();
	table[0] = -1;
	while(i < m) {
		while(j >= 0 && pattern[i] != pattern[j]) j = table[j];
		i++, j++;
		table[i] = j;
	}
	return table;
}

vi find(string& text, string& pattern) {
	vi matches;
	int i = 0, j = 0, n = text.length(), m = pattern.length();
	vi table = buildTable(pattern);
	while(i < n) {
		while(j >= 0 && text[i] != pattern[j]) j = table[j];
		i++, j++;
		if(j == m) {
			matches.pb(i-j);
			j = table[j];
		}
	}
	return matches;
}

```
