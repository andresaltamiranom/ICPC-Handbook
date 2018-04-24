# Header

The following code can be used as a template when starting a new program. This template facilitates and speeds up the coding process by defining macros that are shorter to write and read when creating code.

```cpp

#include <bits/stdc++.h>
#define _ ios_base::sync_with_stdio(0), cin.tie(0), cout.tie(0);
#define INF 1000000000
#define FOR(i, a, b) for(int i=int(a); i<int(b); i++)
#define FORC(cont, it) for(decltype((cont).begin()) it=(cont).begin(); it!=(cont).end(); it++)
#define pb push_back
#define mp make_pair
#define eb emplace_back
#define fi first
#define se second
#define all(x) (x).begin(), (x).end()
using namespace std;
typedef long long ll;
typedef pair <int, int> ii;
typedef vector<int> vi;
typedef vector<ii> vii;
typedef vector<vi> vvi; 
```

Following is the description for each of these statements.

```cpp

#include <bits/stdc++.h>
```

Header that includes all c++ standard libraries.

```cpp

#define _ ios_base::sync_with_stdio(0), cin.tie(0), cout.tie(0);
```

**ios_base::sync_with_stdio(0):** Disables synchronization between the standard C and C++ streams. This allows the streams to have independent input and output buffers, which makes these operations faster (Input/Output).

**Cin.tie(0), Cout.tie(0):** Unlinks _cin_ from _cout_. Without this option enabled, our program would _flush_ automatically before every I/O operation, which consumes valuable time when running our program.

```cpp

#define INF 1000000000
```

We define a very large number to use as infinite. This is used as a shortcut which is not only shorter, but it also makes our code more readable when we are working with very large upper limits.

```cpp

#define FOR(i, a, b) for(int i=int(a); i<int(b); i++)
```

Shorter way of writing a _for_ loop. Receives as parameters the name of the variable to use as the counter, its initial value, and the upper limit's value. This _for_ loops increments the variable received as parameter by 1 in each iteration. If the _for_ loop requires a different behavior than this (i.e. operations different to adding by 1), the loop will have to be written in its original form.

```cpp

#define FORC(cont, it) for(decltype((cont).begin()) it=(cont).begin(); it!=(cont).end(); it++)
```
Simplified form to write a _for_ loop that will use iterators of structures that are not accessed via its index, such as _map_ or _set_. Receives as parameters the name of the variable to iterate through and the iterator's variable name.

```cpp

#define pb push_back
#define mp make_pair
#define eb emplace_back
#define fi first
#define se second
```
Simplified form to write the instructions to the right hand side of each one.

```cpp

#define all(x) (x).begin(), (x).end()
```

Shorter form to refer to the whole range covered in an iterable structure. The structure must be iterable, you must be able to reference its initial and final iterators through the .begin() and .end() functions, such as in a _vector_.

```cpp

using namespace std;
```

This namespace prevents us from having to type the prefix _std::_ to all C++ standard library elements that we use in our program.

```cpp

typedef long long ll;
typedef pair <int, int> ii;
typedef vector<int> vi;
typedef vector<ii> vii;
typedef vector<vi> vvi;
```
Creates a shorter alias that can be used anywhere in the program instead of the name of its data type.
