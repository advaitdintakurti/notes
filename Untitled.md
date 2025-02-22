# Heaps in C

## Definition of a Heap

A **heap** is a complete binary tree that satisfies the **heap property**:

- **Max heap**: The value of each node is greater than or equal to the values of its children.
- **Min heap**: The value of each node is less than or equal to the values of its children.

A **complete binary tree** ensures all levels are completely filled except possibly the last level, which is filled from left to right.

## Heap Representation in Arrays

A heap can be stored efficiently using an array:

- **Parent of node at index `i`**: `⌊(i - 1) / 2⌋`
- **Left child of node at index `i`**: `2i + 1`
- **Right child of node at index `i`**: `2i + 2`

## Heap Operations

### Insertion

1. Insert the new node at the next available position.
2. **Sift-up** (bubble up) the node by swapping it with its parent until the heap property is restored.
3. **Time complexity**: O(log⁡n)O(\log n).

### Deletion (Removing Root)

4. Swap the root with the last element.
5. Remove the last element.
6. **Sift-down** (heapify) the new root until the heap property is restored.
7. **Time complexity**: O(log⁡n)O(\log n).

## Heap Construction

### Naïve Method (Repeated Insertions)

8. Insert elements one by one into an initially empty heap.
9. **Time complexity**: O(nlog⁡n)O(n \log n).

### Optimal Heap Construction (Bottom-Up Heapify)

10. Start from the last internal node (`⌊n/2⌋ - 1`) and apply **sift-down**.
11. **Time complexity**: O(n)O(n).

## Heap Sort Algorithm

12. **Build a max heap** from the array (`O(n)`).
13. **Extract max repeatedly** (swap the root with the last element and heapify) (`O(n \log n)`).
14. **Time complexity**: O(nlog⁡n)O(n \log n) in worst case.
15. **Space complexity**: O(1)O(1) (in-place sorting).

## Applications of Heaps

- **Priority queues** (fast insertion and extraction of max/min).
- **Graph algorithms** (Dijkstra’s and Prim’s algorithms).
- **Median finding** (using min and max heaps).
- **Efficient sorting** (Heap Sort is better than Quick Sort for worst-case scenarios).

## Additional Heap Properties

- **Maximum nodes at height `h`**: `2^h`
- **Height of a heap with `n` nodes**: `⌊log₂ n⌋`
- **Heap can be built in O(n)** using bottom-up approach, even though naive insertion takes O(nlog⁡n)O(n \log n).

## Heap Implementation in C

A heap can be implemented in C using an array:

```c
#include <stdio.h>

void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest])
        largest = left;
    if (right < n && arr[right] > arr[largest])
        largest = right;
    if (largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    for (int i = n - 1; i > 0; i--) {
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        heapify(arr, i, 0);
    }
}

int main() {
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);
    heapSort(arr, n);
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```

This program implements **Heap Sort** using a max heap.

## Questions to Explore

16. What is the minimum height of a heap with `n` nodes?
17. Given an array where each element is at most `k` positions away from its target position, devise an efficient sorting algorithm.
18. Can two different min heaps be formed from the same input sequence? Under what conditions?
19. What is the complexity of inserting `n` elements into an initially empty min heap?

Understanding heaps is crucial for mastering efficient data structures and algorithms!