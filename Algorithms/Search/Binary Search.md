# Binary Search

A general method for searching for an element in an array is to use a for loop that iterates through the elements of the array. The time complexity of this approach is O(n) because, in the worst case, it is necessary to check all elements of the array. If the order of the elements is arbitrary, this is also the best possible approach, because there is no additional information available about where in the array we should search for the element _x_.

However, if the array is sorted, the situation is different. In this case it is possible to perform the search much faster, because the order of the elements in the array guides the search. The following binary search algorithm efficiently searches for an element in a sorted array in O(log(n)) time.

```cpp

const int UPPERBOUND = 0, LOWERBOUND = 1, ANY = 2;
int binarySearch(int array[], int searchValue, int left, int right, int type = ANY) {
	int leftBound = left, rightBound = right;
	while(left <= right) {
        int mid = (left+right)/2;
		if(searchValue > array[mid]) left = mid+1;
		else if (searchValue < array[mid]) right = mid-1;
		else {
            if(type == UPPERBOUND) {
                if(mid == rightBound || array[mid+1] != array[mid])
                    return mid;
                left = mid+1;
            } else if(type == LOWERBOUND) {
                if(mid == leftBound || array[mid-1] != array[mid])
                    return mid;
                right = mid-1;
            } else {
                return mid;
            }
		}
	}
	return -1;
}

```
