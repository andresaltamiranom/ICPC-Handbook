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

KMP uses the information gained by previous character comparisons, especially those that match. KMP _never_ re-compares a character in T that has matched a character in P. However, it works similar to the naïve algorithm if the _first_ character of pattern P and the current character in T is a mismatch. In the example below, comparing P[j] and T[i] and from i = 0 to 13 with j = 0 (the first character of P) is no different than the naïve algorithm.

![KMP](https://i.imgur.com/3tE2jOl.png)

The first character of P mismatches with T[i] from index i = 0 to 13. KMP has to shift the starting index i by +1, just like with naïve matching.

... at i = 14 and j = 0:

![KMP](https://i.imgur.com/1iBurly.png)

... then mismatch at index i = 25 and j = 11

There are 11 matches from index i = 14 to 24, but one mismatch at i = 25 (j = 11). The naïve matching algorithm will inefficiently restart from index i = 15 but KMP can resume from i = 25. This is because the matched characters before the mismatch is "SEVENTY SEV". "SEV" (of length 3) appears as **both** proper suffix and prefix of "SEVENTY SEV". This "SEV" is also called as the **border** of "SEVENTY SEV". We can safely skip index i = 14 to 21: "SEVENTY " (notice the trailing space) in "SEVENTY SEV" as it will not match again, but we cannot rule out the possibility that the next match starts from the second "SEV". So, KMP resets j back to 3, skipping 11 - 3 = 8 characters of "SEVENTY " (notice the trailing space), while i remains at index 25. This is the major difference between KMP and the naïve matching algorithm.

... at i = 25 and j = 3 (This makes KMP efficient) ...

![KMP](https://i.imgur.com/24Iumqe.png)

... then immediate mismatch at index i = 25, j = 3

This time, the prefix of P before mismatch is "SEV", but it does not have a border, so KMP resets j back to 0 (or in other words, restart matching pattern P from the front again).

... mismatches from i = 25 to i = 29... then matches from i = 30 to i = 42...

![KMP](https://i.imgur.com/4eBv2ya.png)

This is a match, so P = "SEVENTY SEVEN" is found at index i = 30. After this, KMP knows that "SEVENTY SEVEN" has "SEVEN" (of length 5) as border, so KMP resets j back to 5, effectively skipping 13 - 5 = 8 characters of "SEVENTY " (notice the trailing space), immediately resumes the search from i = 43, and gets another match. This is efficient.

... at i = 43 and j = 5, we have matches from i = 43 to i = 50...

![KMP](https://i.imgur.com/c3ouDnv.png)

To achieve such speed up, KMP has to preprocess the pattern string and get the 'reset table'. If given pattern string P = "SEVENTY SEVEN", then the table will look like this:

![KMP](https://i.imgur.com/nTZFkwY.png)

This means that if a mismatch happens in j = 11 in the example above (i.e. after finding matches for "SEVENTY SEV"), then we know that we have to re-try matching P from index j = table[11] = 3, i.e. KMP now assumes that it has matched only the first three characters of "SEVENTY SEV", which is "SEV", because the next match can start with that prefix "SEV". The relatively short implementation of the KMP algorithm with comments is shown below. This implementation has a time complexity of O(N+M).

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
