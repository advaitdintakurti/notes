
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

#### Example

```c
Node* root = NULL;
root = insert(root, 20);
insert(root, 10);
insert(root, 30);
insert(root, 5);
insert(root, 15);
insert(root, 25);
insert(root, 35);
```

**Tree Structure:**

```
        20
       /  \
     10    30
    /  \   /  \
   5   15 25  35
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

## **Finding Predecessor and Successor**

For a given node, its:

- **Predecessor** is the largest value **smaller** than the node.
- **Successor** is the smallest value **greater** than the node.

```c
Node* predecessor(Node* root, int key) {
    Node* pred = NULL;
    while (root) {
        if (key > root->key) {
            pred = root;
            root = root->right;
        } else {
            root = root->left;
        }
    }
    return pred;
}

Node* successor(Node* root, int key) {
    Node* succ = NULL;
    while (root) {
        if (key < root->key) {
            succ = root;
            root = root->left;
        } else {
            root = root->right;
        }
    }
    return succ;
}
```


**BST Traversal in Ascending Order:**  
`5 → 10 → 15 → 20 → 25 → 30 → 35`

- `predecessor(20) = 15`, `successor(20) = 25`
- `predecessor(30) = 25`, `successor(30) = 35`
- `predecessor(5) = NULL`, `successor(5) = 10`

---

## **Deleting a Node in BST**

1. **Leaf Node:** Delete it directly.
2. **One Child:** Replace with the child.
3. **Two Children:** Replace with **inorder successor** and delete successor.

```c
Node* deleteNode(Node* root, int key) {
    if (!root) return root;

    if (key < root->key) root->left = deleteNode(root->left, key);
    else if (key > root->key) root->right = deleteNode(root->right, key);
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

### **Example Deletion**

```c
root = deleteNode(root, 20);
```

**Tree after deleting 20:**

```
        25
       /  \
     10    30
    /  \      \
   5   15      35
```

---

## **Tree Traversals**

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
```

### **Example Output**

```c
inorder(root);  // Output: 5 10 15 25 30 35
```

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

## **Conclusion**

BSTs allow efficient insertion, deletion, and lookup. **Predecessor/successor** are useful for **sorted-order operations**, and **LCA** is essential for **relationship queries**. Balanced BSTs like **AVL Trees** maintain performance across different insertions.