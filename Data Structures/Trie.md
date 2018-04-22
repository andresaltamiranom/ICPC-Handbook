# Trie

A trie is a rooted tree that maintains a set of strings. Each string in the set is stored as a chain of characters that starts at the root. If two strings have a common prefix, they also have a common chain in the tree.

For example, consider the following trie:

![Trie](https://i.imgur.com/Qb7dXa1.png)

This trie corresponds to the set {CANAL, CANDY, THE, THERE}. The character \* in a node means that a string in the set ends at the node. Such a character is needed, because a string may be a prefix of another string. For example, in the above trie,
_THE_ is a prefix of _THERE_.

We can check in O(n) time whether a trie contains a string of length _n_, because we can follow the chain that starts at the root node. We can also add a string of length _n_ to the trie in O(n) time by first following the chain and then adding new
nodes to the trie if necessary.

Using a trie, we can find the longest prefix of a given string such that the prefix belongs to the set. Moreover, by storing additional information in each node, we can calculate the number of strings that belong to the set and have a given string as a prefix.

A common application of a trie is storing a predictive text or autocomplete dictionary, such as found on a mobile phone's keyboard. Such applications take advantage of a trie's ability to quickly search for, insert, and delete entries. Tries are also well suited for implementing approximate matching algorithms, including those used in spell checking.

```cpp

#define ALPHABET_SIZE 52
int getIndex(char c) {
	if(c >= 'A' && c <= 'Z')
		return c-'A';
	return c-'a'+26;
}

struct Trie {
	int words, prefixes;
	Trie *edges[ALPHABET_SIZE];
	Trie() : words(0), prefixes(0) { FOR(i, 0, ALPHABET_SIZE) edges[i] = 0; }
	~Trie(){ FOR(i, 0, ALPHABET_SIZE) if(edges[i]) delete edges[i]; }
	void insert(char *word, int pos = 0) {
		if(word[pos] == 0) {
			words++;
			return;
		}
		prefixes++;
		int index = getIndex(word[pos]);
		if(edges[index] == 0)
			edges[index] = new Trie;
		edges[index]->insert(word, pos+1);
	}
	int countWords(char *word, int pos = 0) {
		if(word[pos] == 0)
			return words;
		int index = getIndex(word[pos]);
		if(edges[index]==0)
			return 0;
		return edges[index]->countWords(word, pos+1);
	}
	int countPrefix(char *word, int pos = 0) {
		if(word[pos] == 0)
			return prefixes;
		int index = getIndex(word[pos]);
		if(edges[index] == 0)
			return 0;
		return edges[index]->countPrefix(word, pos+1);
	}
};

```
