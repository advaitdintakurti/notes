Heap Sort is a comparison-based sorting algorithm that uses a binary heap data structure.

### Steps

1. **Build a Max Heap**: Convert the given array into a max heap. This is done by calling `heapify` on all non-leaf nodes, starting from the last non-leaf node (at index n/2 - 1).

2. **Heap Sort Process**:
    - Swap the root (largest element) with the last element.
    - Reduce the heap size.
    - Restore heap property by calling `heapify` on the new root.
    - Repeat until all elements are sorted.

### Heapify Function

The `heapify` function ensures that a subtree rooted at index `i` satisfies the max heap property. If a child node is larger than the parent, swap them and recursively call `heapify` on the affected subtree.

### Implementation in C

```c
#include <stdio.h>

// Function to heapify a subtree rooted at index i
void heapify(int arr[], int n, int i) {
    int largest = i; // Initialize largest as root
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    // Check if left child exists and is larger than root
    if (left < n && arr[left] > arr[largest])
        largest = left;

    // Check if right child exists and is larger than largest so far
    if (right < n && arr[right] > arr[largest])
        largest = right;

    // Swap and continue heapifying if root is not largest
    if (largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        heapify(arr, n, largest);
    }
}

// Main Heap Sort function
void heapSort(int arr[], int n) {
    // Build max heap (rearrange array)
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        // Move current root to end
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;

        // Call heapify on the reduced heap
        heapify(arr, i, 0);
    }
}

// Function to print an array
void printArray(int arr[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

// Driver code
int main() {
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);

    heapSort(arr, n);
    printf("Sorted array: ");
    printArray(arr, n);

    return 0;
}
```

### Explanation of Code

1. **`heapify(arr, n, i)`**:
    - Recursively ensures that a subtree rooted at `i` maintains the heap property.
    - Swaps the root with its largest child if necessary and calls `heapify` on the affected subtree.
    
2. **`heapSort(arr, n)`**:
    - First, it constructs a max heap by calling `heapify` from the last non-leaf node up to the root.
    - Then, it repeatedly swaps the root with the last element, reduces the heap size, and restores heap order.
    
3. **Time Complexity**:
    - Building the max heap takes O(n).
    - Extracting the maximum element n times and calling `heapify` each time costs O(log⁡n).
    - Total complexity: O(nlog⁡n).
