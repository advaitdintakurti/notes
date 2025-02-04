## Bucket Sort

Bucket sort distributes elements into a number of buckets. Each bucket corresponds to a range of values. Elements are distributed based on their values, sorted within the buckets, and then concatenated to produce the final sorted array.

> **Algorithm:**
>     Initialize an array `count` of size `m` (number of buckets) to zero.
>     For each element `a[i]` in the input array:
>         Increment `count[a[i]]`
>    For each index `i`, append `count[i]` copies of `i` to the sorted output

#### Complexity Analysis

**Time Complexity:** `O(m+n)`
> `m`  = range of input values (number of buckets).
> `n` = number of elements to sort.

Optimal when `m = O(n)`, making the algorithm `O(n)`.

**Space Complexity:** `O(m)` (for the`count` array) 


## Radix Sort

Radix sort is a digit-based sorting algorithm that processes digits from least significant to most significant (LSD-first)

> Uses **stable sorting** (preserves relative order of elements)

Radix sort passes the numbers through multiple passes of *bucket sort*, digit by digit.

> **Algorithm:**
>     For each digit (starting from LSD):
>         Initialize `b` buckets (e.g., 0-9 for base 10).
>         Distribute numbers into buckets based on the current digit.
>         Collect numbers from buckets in order.

![[radixsort.png|500]]

#### Complexity Analysis

**Time Complexity:** `O(p(n + b))`
> `p` = number of digits (passes)
> `n` = number of elements
> `b` = number of buckets (typically 10 for base 10).

If `b = O(n)`, time complexity becomes `O(n)`.

**Space Complexity:** `O(b+n)`
> `b` = space for buckets.
> `n` = space for temporary storage of elements.

#### Pro/Con Analysis

**Advantages:**
- Efficient for data with a small number of digits.   
- Stable sorting ensures correct order of duplicate keys.  

**Limitations:**
- Requires knowledge of the number of digits.
- Not comparison-based, so less flexible for arbitrary data.
- Inefficient for very large ranges without dense populations.

### Implementation

```c
#include <stdio.h>

// A utility function to get the maximum value in arr[]
int getMax(int arr[], int n) {
    int mx = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > mx)
            mx = arr[i];
    return mx;
}

// A function to do counting sort of arr[] 
// according to the digit represented by exp
void countSort(int arr[], int n, int exp) {
    int output[n]; // Output array
    int count[10] = {0}; // Initialize count array as 0

    // Store count of occurrences in count[]
    for (int i = 0; i < n; i++)
        count[(arr[i] / exp) % 10]++;

    // Change count[i] so that count[i] now 
    // contains actual position of this digit
    // in output[]
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    // Build the output array
    for (int i = n - 1; i >= 0; i--) {
        output[count[(arr[i] / exp) % 10] - 1] = arr[i];
        count[(arr[i] / exp) % 10]--;
    }

    // Copy the output array to arr[], 
    // so that arr[] now contains sorted 
    // numbers according to current digit
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

// The main function to sort arr[] of size n using Radix Sort
void radixSort(int arr[], int n) {
  
    // Find the maximum number to know 
    // the number of digits
    int m = getMax(arr, n); 

    // Do counting sort for every digit
    // exp is 10^i where i is the current 
    // digit number
    for (int exp = 1; m / exp > 0; exp *= 10)
        countSort(arr, n, exp);
}
```