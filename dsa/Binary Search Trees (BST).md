
A **Binary Search Tree (BST)** is a binary tree with the following properties:

- Each node contains a key, and all keys in the **left subtree** are smaller.
- All keys in the **right subtree** are greater.
- No duplicate keys (in standard BSTs).

BSTs allow efficient **O(log n)** insertion, search, and deletion, making them ideal for dynamic sorted data storage.

### BST Node Structure

```c
typedef struct Node {
    int key;
    struct Node *left, *right;
} Node;
```

### Creating a New Node

```c
#include <stdlib.h>

Node* newNode(int key) {
    Node* node = (Node*)malloc(sizeof(Node));
    node->key = key;
    node->left = node->right = NULL;
    return node;
}
```

### Insertion in BST

We recursively compare the key with the current node and insert it into the left or right subtree accordingly.

```c
Node* insert(Node* root, int key) {
    if (!root) return newNode(key);
    if (key < root->key) root->left = insert(root->left, key);
    else if (key > root->key) root->right = insert(root->right, key);
    return root;
}
```

---

### Searching

```c
Node* search(Node* root, int key) {
    if (!root || root->key == key) return root;
    return (key < root->key) ? search(root->left, key) : search(root->right);
}
```

---

### Finding Minimum and Maximum

- **Minimum:** Leftmost node
- **Maximum:** Rightmost node

```c
Node* findMin(Node* root) {
    while (root && root->left) root = root->left;
    return root;
}

Node* findMax(Node* root) {
    while (root && root->right) root = root->right;
    return root;
}
```

---

## **Deleting a Node in BST**

1. **Leaf Node:** Delete it directly.
2. **One Child:** Replace with the child.
3. **Two Children:** Replace with **inorder successor** and delete successor.

```c
Node* deleteNode(Node* root, int key) {
    if (!root) return root;

    // search part
    if (key < root->key) root->left = deleteNode(root->left, key);
    else if (key > root->key) root->right = deleteNode(root->right, key);
    // deletion part
    else {
        if (!root->left) {
            Node* temp = root->right;
            free(root);
            return temp;
        }
        if (!root->right) {
            Node* temp = root->left;
            free(root);
            return temp;
        }

        Node* temp = findMin(root->right);
        root->key = temp->key;
        root->right = deleteNode(root->right, temp->key);
    }
    return root;
}
```

---

## **Tree Traversals

4. **Inorder (Left, Root, Right)** → Sorted Order
5. **Preorder (Root, Left, Right)**
6. **Postorder (Left, Right, Root)**

```c
#include <stdio.h>

void inorder(Node* root) {
    if (!root) return;
    inorder(root->left);
    printf("%d ", root->key);
    inorder(root->right);
}

void preorder(Node* root) {
    if (!root) return;
    printf("%d ", root->key);
    preorder(root->left);
    preorder(root->right);
}

void postorder(Node* root) {
    if (!root) return;
    postorder(root->left);
    postorder(root->right);
    printf("%d ", root->key);
}
```


## **Balancing a BST**

- Inorder traversal to make a sorted array
- Construct a new (balanced) BST from the elements of the sorted array

---

## **Finding Lowest Common Ancestor (LCA)**

LCA of two nodes is the deepest node that has both in its subtrees.

```c
Node* LCA(Node* root, int n1, int n2) {
    if (!root) return NULL;
    if (n1 < root->key && n2 < root->key) return LCA(root->left, n1, n2);
    if (n1 > root->key && n2 > root->key) return LCA(root->right, n1, n2);
    return root;
}
```

### **Example**

```c
Node* ancestor = LCA(root, 5, 15);  // Returns node with key 10
```

---

## **Checking if a Tree is a BST**

A valid BST must satisfy `min < root < max` at every node.

```c
int isBSTUtil(Node* root, int min, int max) {
    if (!root) return 1;
    if (root->key <= min || root->key >= max) return 0;
    return isBSTUtil(root->left, min, root->key) && isBSTUtil(root->right, root->key, max);
}

int isBST(Node* root) {
    return isBSTUtil(root, -2147483648, 2147483647);
}
```

---

## Advantages of Binary Search Tree ****(BST)****:

- **Efficient searching:** O(log n) time complexity for searching with a self balancing BST
- **Ordered structure:** Elements are stored in sorted order, making it easy to find the next or previous element
- **Dynamic insertion and deletion:** Elements can be added or removed efficiently
- **Balanced structure:** Balanced BSTs maintain a logarithmic height, ensuring efficient operations
- **Doubly Ended Priority Queue**: In BSTs, we can maintain both maximum and minimum efficiently

## Disadvantages of Binary Search Tree ****(BST)****:

- **Not self-balancing:** Unbalanced BSTs can lead to poor performance
- **Worst-case time complexity:** In the worst case, BSTs can have a linear time complexity for searching and insertion
- **Memory overhead:** BSTs require additional memory to store pointers to child nodes
- **Not suitable for large datasets:** BSTs can become inefficient for very large datasets
- **Limited functionality:** BSTs only support searching, insertion, and deletion operations

---
Next: [[Fenwick Trees]]