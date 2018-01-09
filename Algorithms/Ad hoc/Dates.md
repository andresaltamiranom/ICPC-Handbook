# Dates

### toJulian

```cpp

int toJulian(int day, int month, int year) {
	return 1461 * (year + 4800 + (month - 14) / 12) / 4 + 367 * (month - 2 -
		(month - 14) / 12 * 12) / 12 - 3 * ((year + 4900 + (month - 14) / 12)
		/ 100) / 4 + day - 32075;
}

```

**Input:** Three integers. A day, a month, and a year.

**Output:** An integer, the Julian date represented by the given day, month, and year.

Converts a Gregorian date (day, month, and year) to a Julian date. Julian dates are simply a continuous count of days and fractions since noon (12:00 PM) Universal Time on January 1, 4713 BC (on the Julian calendar).

### toGregorian

```cpp

void toGregorian(int julian, int &day, int &month, int &year) {
	int x, n, i, j;
	x = julian + 68569;
	n = 4 * x / 146097;
	x -= (146097 * n + 3) / 4;
	i = (4000 * (x + 1)) / 1461001;
	x -= 1461 * i / 4 - 31;
	j = 80 * x / 2447;
	day = x - 2447 * j / 80;
	x = j / 11;
	month = j + 2 - 12 * x;
	year = 100 * (n - 49) + i + x;
}

```

**Input:** An integer representing a Julian date. Three integers (day, month, and year) are passed by reference.

**Output:** None. However, _day_, _month_, and _year_ passed by reference will hold the Gregorian date equivalent to the Julian date passed as input.

Converts a Julian date to a Gregorian date (day, month, year).

### isLeap

```cpp

bool isLeap(int year) { return (year%4 == 0 && year%100 != 0) || year%400 == 0; }

```

**Input:** An integer representing a year.

**Output:** A boolean value, whether the given year is leap or not.

A year is leap if it is divisible by 400 or if it is divisible by 4 but not by 100.
