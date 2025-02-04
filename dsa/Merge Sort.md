Merge sort is a sorting algorithm that uses the divide-and-conquer strategy:
1. Divide the array into two halves.
2. Recursively sort each half.
3. Merge the sorted halves.

![[mergesort.png|400]]

> **Algorithm:**
>     If array size is 1:
>         Array is already sorted
>     Else:
>         Break into left and right halves
>         Merge sort each half individually

### Implementation

Top down implementation of merge sort.

- Recursively splits the array until single elements are reached.
- Merges sorted arrays back up.

#### Merge function

Combines two sorted subarrays into a single sorted array:

```c
void merge(int arr[], int start, int mid, int end) {
    int size = end - start + 1;
    int temp[size]; // Single temporary array
    int i = start, j = mid + 1, k = 0;

    // Merge elements from both halves into the temporary array
    while (i <= mid && j <= end) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }

    // Copy remaining elements from the left half
    while (i <= mid) {
        temp[k++] = arr[i++];
    }

    // Copy remaining elements from the right half
    while (j <= end) {
        temp[k++] = arr[j++];
    }

    // Copy sorted elements back to the original array
    for (i = 0; i < size; i++) {
        arr[start + i] = temp[i];
    }
}
```

#### Merge sort function

Recursively sorts the array.

```c
void mergesort(int arr[], int left, int right) {
    if (left >= right) return;

    int mid = left + (right - left) / 2;
    mergesort(arr, left, mid);
    mergesort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}
```


### Complexity Calculations:
```
T(n) = T(n/2) + T(n/2) + kn
        = T(n/4) + T(n/4) + k(n/2) + T(n/4) + T(n/4) + k(n/2) + kn
        = 4T(n/4) + 2kn
        = 8T(n/8) + 3kn
        = 16T(n/16) + 4kn
        ...
        = 2^i * T(n/2^i) + ikn
        ...
        = nT(n/n) + knlogn
        = nT + knlogn
        ~ O(nlogn)
```

## Key Properties

- **Time Complexity**: `O(nlogn⁡)` in all cases (best, worst, average).
- **Space Complexity**: `O(n)`
- **Stability**: Preserves the order of equal elements.
- **Not In-Place**: Requires extra memory for temporary arrays.
