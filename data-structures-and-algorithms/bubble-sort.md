<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Data Structures & Algorithms</a> > Bubble Sort

# Bubble Sort



## Bubble Sort ( T = O(n^2^) )

* A simple sorting algorithm that repeatedly steps through a list of elements, compares adjacent elements, and swaps them if they  are in the wrong order. This process is repeated for each pair of adjacent elements until the entire list is sorted.
* Gets its name from the way smaller elements "bubble" to the beginning of the list with each pass.
* Time complexity: T = O(n^2^)  (Inefficient for large lists)



## Code

```cpp
#include <iostream>

using namespace std;

void bubbleSort(int arr[], int size)
{
    for (int i = size - 1; i > 0; i--)
    {
        for (int j = 0; j < i; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                // Swap
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main(void)
{
    int arr[] = {6, 4, 2, 1, 5, 3};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    bubbleSort(arr, size);
    
    for (auto val : arr)
        cout << val << " ";
	
    cout << endl;
    
    return 0;
}
```

```plain
1 2 3 4 5 6
```
