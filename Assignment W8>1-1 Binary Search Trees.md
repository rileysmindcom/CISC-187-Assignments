# Assignment W8>1/1 Binary search trees
## Lab Objective
The goal of this lab is understanding how a Binary Search Tree (BST) uses dynamically allocated nodes, and pointers to store, and arrange data. Without utilizing std::set or std::map. To show how we can discover the lowest, and maximum values, execute in-order, reorder, and post-order, traversals, insert values, as we preserve the BST ordering property, delete nodes with zero, one or two children, and then release dynamically allocated memory. To look at how the tree height influences its performance connecting skewed, and balanced trees.

## Assignment Code
```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

class BinarySearchTree {
private:
    Node* root;

    Node* insert(Node* node, int value) {
        if (node == nullptr) {
            return new Node(value);
        }

        if (value < node->data) {
            node->left = insert(node->left, value);
        }
        else if (value > node->data) {
            node->right = insert(node->right, value);
        }

        return node;
    }

    bool search(Node* node, int value) const {
        if (node == nullptr) {
            return false;
        }

        if (value == node->data) {
            return true;
        }

        if (value < node->data) {
            return search(node->left, value);
        }

        return search(node->right, value);
    }

    void inorder(Node* node) const {
        if (node == nullptr) return;
        inorder(node->left);
        cout << node->data << ' ';
        inorder(node->right);
    }

    void preorder(Node* node) const {
        if (node == nullptr) return;
        cout << node->data << ' ';
        preorder(node->left);
        preorder(node->right);
    }

    void postorder(Node* node) const {
        if (node == nullptr) return;
        postorder(node->left);
        postorder(node->right);
        cout << node->data << ' ';
    }

    Node* findMin(Node* node) const {
        if (node == nullptr) return nullptr;
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    Node* findMax(Node* node) const {
        if (node == nullptr) return nullptr;
        while (node->right != nullptr) {
            node = node->right;
        }
        return node;
    }

    Node* remove(Node* node, int value) {
        if (node == nullptr) {
            return nullptr;
        }

        if (value < node->data) {
            node->left = remove(node->left, value);
        }
        else if (value > node->data) {
            node->right = remove(node->right, value);
        }
        else {

            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }

            if (node->left == nullptr) {
                Node* replacement = node->right;
                delete node;
                return replacement;
            }

            if (node->right == nullptr) {
                Node* replacement = node->left;
                delete node;
                return replacement;
            }

            Node* successor = findMin(node->right);
            node->data = successor->data;
            node->right = remove(node->right, successor->data);
        }

        return node;
    }

    void deleteTree(Node* node) {
        if (node == nullptr) return;
        deleteTree(node->left);
        deleteTree(node->right);
        delete node;
    }

public:
    BinarySearchTree() : root(nullptr) {}

    BinarySearchTree(const BinarySearchTree&) = delete;
    BinarySearchTree& operator=(const BinarySearchTree&) = delete;

    ~BinarySearchTree() {
        deleteTree(root);
    }

    bool empty() const {
        return root == nullptr;
    }

    void insert(int value) {
        root = insert(root, value);
    }

    bool search(int value) const {
        return search(root, value);
    }

    bool remove(int value) {
        if (!search(value)) {
            return false;
        }
        root = remove(root, value);
        return true;
    }

    void inorder() const {
        inorder(root);
        cout << '\n';
    }

    void preorder() const {
        preorder(root);
        cout << '\n';
    }

    void postorder() const {
        postorder(root);
        cout << '\n';
    }

    int minimum() const {
        Node* result = findMin(root);
        if (result == nullptr) {
            throw runtime_error("Cannot find minimum: tree is empty.");
        }
        return result->data;
    }

    int maximum() const {
        Node* result = findMax(root);
        if (result == nullptr) {
            throw runtime_error("Cannot find maximum: tree is empty.");
        }
        return result->data;
    }
};

int main() {
    BinarySearchTree tree;

    cout << boolalpha;
    cout << "Initially empty: " << tree.empty() << '\n';

    int values[] = { 50, 30, 70, 20, 40, 60, 80 };
    for (int value : values) {
        tree.insert(value);
    }

    tree.insert(40); 

    cout << "After insertions, inorder: ";
    tree.inorder();

    cout << "Preorder: ";
    tree.preorder();

    cout << "Postorder: ";
    tree.postorder();

    cout << "Search 50 (root): " << tree.search(50) << '\n';
    cout << "Search 20 (leaf): " << tree.search(20) << '\n';
    cout << "Search 30 (internal node): " << tree.search(30) << '\n';
    cout << "Search 99 (missing): " << tree.search(99) << '\n';

    cout << "Minimum: " << tree.minimum() << '\n';
    cout << "Maximum: " << tree.maximum() << '\n';

    cout << "\nRemove leaf 20: " << tree.remove(20) << '\n';
    cout << "Inorder: ";
    tree.inorder();

    cout << "Remove node 30 (now has one child): " << tree.remove(30) << '\n';
    cout << "Inorder: ";
    tree.inorder();

    cout << "Remove root 50 (two children): " << tree.remove(50) << '\n';
    cout << "Inorder: ";
    tree.inorder();

    cout << "Search deleted 50: " << tree.search(50) << '\n';
    cout << "Search remaining 60: " << tree.search(60) << '\n';

    cout << "\nFinal minimum: " << tree.minimum() << '\n';
    cout << "Final maximum: " << tree.maximum() << '\n';

    return 0;
}
```
## Analysis, and Reflection
### Step 1: Binary Search Tree Validity
The left sub-tree of a Binary Search Tree (BST) needs to be smaller than those in the parent, while those in the right sub-tree must be greater. So the tree's execution with these rules are ascertained by examining the positions of each node. This helped me recognize how BST arranges values. Discovering why every node needs to look at the BST ordering rules.

### Step 2: Constructing a BST
A BST with smaller values on the left, and larger values on the right are produced by inserting items in a specific order. This order is where values are added to determine the final tree structure. Which, helped me understand how a tree's shape is influenced by its insertion order. Making it easier to recognize how BSTs arrange data.

### Step 3: Searching a BST
We look at searching travels from the left to right based on the outcome of comparing a target value with the current node. Whether the value is located or a null pointer is reached, and the search is terminated. Reflecting this gave me a way to look at how comparisons direct, and search procedures work. Discovering that search efficiency is affected by the structure of the tree.

### Step 4: Implementing the BST Class
The BST class uses nodes, and pointers to arrange the tree's operations, and data. The application can dynamically insert, search, and manage values because of its structure. I discovered how pointers, and classes all combine to form dynamic data structures.

### Step 5: Inserting Values
Before an empty position is found, the insertion operation compares new values with nodes that already exist. After every insertion, this maintains the BST property. Then looking at how to add values without violating the ordering guidelines of the tree. Which, gave me an understanding of how pointers, and comparisons are observed through these practices.

### Step 6: Searching for Values
We can compare each node in the search function to determine the right route. Because the tree's height can be affected by how effective it is. Then I discovered that searching does not need to examine each node through a BST. But then I see how data structures can increase effiecency.

### Step 7: Tree Traversals
Before traversals move through a BST's nodes from smallest to greatest. The Preorder, and post-order traversals are helpful for other tree operations because they visit nodes in different orders. I became more aware of the differences between post-order, pre-order, and in-door traversal. As every technique offered an approach to handle tree data.

### Step 8: Finding Minimum, and Maximum
The leftmost node has the lowest value as the rightmost node has the highest value. We can look at the ordering of BST by the use of these operations to find the values. To discover how to determine the biggest, and smallest numbers without looking at every node. Which, represents this in the BST structure's value.

### Step 9: Deleting Nodes
Depending on whether the node contains zero, one, or two. There are different procedures that needs to be followed before we order to delete it. The inorder successor of a node with two children can take its place while preserving the BST function. This is because different circumstances need to be handles, which is why I discovered that deletion can be difficult versus insertion. But, became easier to look at how to maintain the tree's structure after learning through these situations.

### Step 10: Verifying Deletion.
We can verify that the tree is still valid after nodes are all removed requiring to test different deletion scenarios, attempts to remove values that don't exist should also be handled by the tree. After making these changes to the data structure, I realized how important it is to test the code. And, the verification helps with finding mistakes to ensure the proper operation.

### Step 11: Balanced Skewed Trees
Because of its lower height a balanced BST typically requires fewer comparisons. This is because a skewed BST's structure is similar to a linked list, and may become less efficient. So when I discovered that a tree's performance is directly impact from its shape. We can maintain a tree's equilibrium to improve the effectiveness of data modification, and searches.

### Part 12: Time Complexity
Looking at a balanced BST, search, deletion, and insertion requires O(log n) time. To look at these procedures that require O(N) time in the worst-case skewed BST. So I discovered how the algorithm's efficency is observed by Big O notation. To realize why balanced trees typically work better.

### Part 13: Range Queries
The values that are between a given minimum, and maximum are found by a range query. Exploring brances that cannot contain values in a specific range can be avoided by using the BST structure. Then I viewed that BSTs are capable of retrieving values inside a given range. Which helped explain how tree ordering can improve in the program from data retrieval.

### Part 14: Memory Management
To avoid memory leaks, dynamically allocated nodes need to be handled carefully. Responsible memory management is ensured by deleting nodes, and releasing unused memory. So I observed the need for caution when it comes to handling pointers, and dynamically allocated memory. To look at errors, and resource waste can be avoided with effective memory management.

### Part 15: Main Function
The BST operations are tested from their outcomes, and can be shown by the main function. We first verify that insertion, searching, traversal, and deletion function can be properly supported by running the program. Which, now I have a better understanding of how, and why different functions in C++ can interact with each other. So I can use these ideas that require organizing, and searching data.

## Flowchart
<img width="762" height="320" alt="Binary search trees Diagram" src="https://github.com/user-attachments/assets/f16d67fd-a33f-4d49-ab8e-a7a501c214c2" />

## Challenges
Some of the challenges I've experienced were understanding how recursion, and pointers reattach the tree after adding or removing a node. Then I had to manage three deletion scenarios to determine the in-order successor, which this is when a node has two children. But, first I had to confirm that the removed values could no longer be located, and that the inorder traversal remained sorted. And, at last was the destructor that releases the children before removing each parent, which was necessary to manage the dynamically generated nodes.

## Assignment Video Link
