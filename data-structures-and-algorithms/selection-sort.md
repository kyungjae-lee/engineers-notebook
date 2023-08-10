<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Data Structures & Algorithms</a> > Selection Sort

# Selection Sort



## Selection Sort ( T = O(n^2^) )

* A sorting algorithm that iteratively selects the smallest (or largest) element from an unsorted portion of the list and places it at the beginning (or end) of the sorted portion. This process is repeated until the entire list is sorted. 

### Algorithm

1. **Input:** An array of elements to be sorted.
2. **Loop:** Iterate through the array from the beginning to the second-to-last element.
3. **Find Minimum:** For each iteration, find the index of the smallest element in the remaining unsorted portion of the array (starting from the current iteration index).
4. **Swap:** Swap the element at the current iteration index with the element at the index of the smallest element found in step 3.
5. **Repeat:** Continue this process, incrementing the iteration index, until the entire array is sorted.

### Pseudo-code

```plain
SelectionSort(arr):
	n = length(arr)
	for i from 0 to n-2:
		minIndex = i
		for j from i+1 to n-1:
			if arr[j] < arr[minIndex]:
				minIndex = j
		if minIndex != i:
			swap(arr[i], arr[minIndex])
```



## Code

```cpp
#include <iostream>

using namespace std;

void selectionSort(int arr[], int size)
{
    for (int i = 0; i < size; i++)
    {
        int minIndex = i;
        for (int j = i + 1; j < size; j++)
        {
            if (arr[j] < arr[minIndex])
                minIndex = j;
        }
        if (minIndex != i)
        {
            int temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }
}

int main(void)
{
    int arr[] = {6, 4, 2, 1, 5, 3};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    selectionSort(arr, size);
    
    for (auto val : arr)
        cout << val << " ";
	
    cout << endl;
    
    return 0;
}
```

```plain
1 2 3 4 5 6
```
