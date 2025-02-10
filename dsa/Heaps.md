A **heap** is a specialized **binary tree** that satisfies the **heap property**. It is commonly used in priority queues and sorting algorithms like Heap Sort. A heap is a **complete binary tree**, meaning all levels are completely filled except possibly the last level, which is filled from left to right.

There are two types of heaps:

1. **Max Heap**: The key at each parent node is **greater than or equal to** the keys of its children. The largest element is always at the root.
2. **Min Heap**: The key at each parent node is **less than or equal to** the keys of its children. The smallest element is always at the root.

Heap operations typically involve **insertion**, **deletion**, and **heapification**, which maintain the heap structure.

### **Heap Representation in an Array**

Since a heap is a complete binary tree, it is efficiently stored as an **array** where:

- The **root** is at index **0**.
- The **left child** of node at index `i` is at index `2*i + 1`.
- The **right child** of node at index `i` is at index `2*i + 2`.
- The **parent** of node at index `i` is at index `(i - 1) / 2`.

This structure allows easy access to children and parents without using pointers.

### **Example of a Max Heap**

```
         16
       /    \
      14     10
     /  \   /  \
    8    7 9    3
   / \
  2   4
```

**Array Representation**:  
`[16, 14, 10, 8, 7, 9, 3, 2, 4]`

### **Heap Operations**

1. **Heapify**: Maintains the heap property by moving a node down the tree until the max heap (or min heap) condition is restored.
2. **Build Heap**: Converts an arbitrary array into a heap using `heapify` in O(n)O(n) time.
3. **Extract Max/Min**: Removes the root and reorders the heap using `heapify`, taking O(log⁡n)O(\log n).
4. **Insert**: Adds an element at the end and moves it up the tree if necessary, taking O(log⁡n)O(\log n).

These operations ensure efficient sorting in **Heap Sort** by repeatedly extracting the max element and rebuilding the heap.

---
Next: [[Heap Sort]]