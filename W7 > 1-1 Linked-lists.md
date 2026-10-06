# Assignment W7 > 1-1 Linked-lists

## Lab Objective
The goal of this lab is to understand how to use pointers to implement a singly linked list with a data structure consisting nodes with data, and a pointer to the next node called a linked list. A linked list does not require its nodes to be kept in contiguous memory comparing to an array. While, each node can be linked to another node by a pointer dynamically allocated. This lab has taught me how to define a node using a head pointer to track the first node dynamically, deallocate nodes, allocate, traverse a list, search for a value, insert nodes at the beginning, and end, insert a node after a specific value, and safely delete nodes. I then compared linked lists with arrays, and vectors to examine Big-O complexity of linked-list operations. Then seeing how pointers manage a linked list, and its structure seeing why some operations can be effective compared to others. Instead of using a standard library linked list classes, I focused on creating a linked-list operation.

## Assignment Code
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

class LinkedList {
private:
    Node* head;

public:
    
    LinkedList() {
        head = nullptr;
    }

    bool empty() const {
        return head == nullptr;
    }

    void display() const {
        Node* temp = head;

        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->next;
        }

        cout << endl;
    }

    bool search(int value) const {
        Node* temp = head;

        while (temp != nullptr) {
            if (temp->data == value) {
                return true;
            }

            temp = temp->next;
        }

        return false;
    }

    void insertFront(int value) {
        Node* newNode = new Node{ value, nullptr };

        newNode->next = head;
        head = newNode;
    }

    void insertBack(int value) {
        Node* newNode = new Node{ value, nullptr };

        if (head == nullptr) {
            head = newNode;
            return;
        }

        Node* temp = head;

        while (temp->next != nullptr) {
            temp = temp->next;
        }

        temp->next = newNode;
    }

    bool insertAfter(int target, int value) {
        Node* temp = head;

        while (temp != nullptr) {
            if (temp->data == target) {
                Node* newNode = new Node{ value, nullptr };

                newNode->next = temp->next;
                temp->next = newNode;

                return true;
            }

            temp = temp->next;
        }

        return false;
    }

    bool remove(int value) {
        if (head == nullptr) {
            return false;
        }

        if (head->data == value) {
            Node* nodeToDelete = head;
            head = head->next;

            delete nodeToDelete;
            return true;
        }

        Node* temp = head;

        while (temp->next != nullptr) {
            if (temp->next->data == value) {
                Node* nodeToDelete = temp->next;

                temp->next = nodeToDelete->next;

                delete nodeToDelete;
                return true;
            }

            temp = temp->next;
        }

        return false;
    }

    ~LinkedList() {
        Node* temp = head;

        while (temp != nullptr) {
            Node* nodeToDelete = temp;
            temp = temp->next;

            delete nodeToDelete;
        }

        head = nullptr;
    }
};

int main() {
    LinkedList list;

    cout << "Is the list empty? "
        << (list.empty() ? "Yes" : "No") << endl;

    list.insertFront(30);
    list.insertFront(20);
    list.insertFront(10);

    cout << "After inserting at the front: ";
    list.display();

    list.insertBack(40);
    list.insertBack(50);

    cout << "After inserting at the back: ";
    list.display();

    cout << "Search for 30: "
        << (list.search(30) ? "Found" : "Not Found") << endl;

    cout << "Search for 100: "
        << (list.search(100) ? "Found" : "Not Found") << endl;

    if (list.insertAfter(30, 35)) {
        cout << "Inserted 35 after 30." << endl;
    }

    cout << "List after insertAfter: ";
    list.display();

    if (!list.insertAfter(100, 105)) {
        cout << "Could not insert after 100 because it was not found."
            << endl;
    }

    if (list.remove(10)) {
        cout << "Removed 10." << endl;
    }

    cout << "After removing the first node: ";
    list.display();

    if (list.remove(30)) {
        cout << "Removed 30." << endl;
    }

    cout << "After removing a middle node: ";
    list.display();

    if (list.remove(50)) {
        cout << "Removed 50." << endl;
    }

    cout << "After removing the last node: ";
    list.display();

    if (!list.remove(100)) {
        cout << "Could not remove 100 because it was not found."
            << endl;
    }

    cout << "Final list: ";
    list.display();

    return 0;
}
```
## Analysis, and Reflection

### 1. Why linked lists do not require contiguous memory
The nodes do not need to be close proximity to each other in memory because each node is generated independently using.

```cpp
Node* newNode = new Node{value, nullptr};
```
The nodes are connected by the proceeding pointer

### 2. The role of the head pointer
The initial node in the linked list is tracked by the head pointer
```cpp
Node* head;
```
So when the list is empty, head is set to nullptr

```cpp
head = nullptr;
```
### 3. Why traversal is O(N)
Until it locates the required node or reaches the end, the program needs to proceed from one node to the next.
```cpp
while (temp != nullptr) {
  temp = temp->next;
}
```
Since, traversal is O(N) it can visit all N nodes.

### 4. Why insertion at the beginning is O(1)
To add a node at the start, only two pointer modifications are required
```cpp
newNode->next = head;
head = newNode;
```
The operation is O(1) because the program does not have to go through the list.

### 5. Why insertion at the end is O(N) without a tail pointer
The program has to locate the final node, as the software must go through the list.
```cpp
while (temp->next != nullptr) {
  temp = temp->next;
}
```
Then it connects a new node after arriving at the last node.
```cpp
temp->next = newNode;
```
So the final insertion is O(N)

### 6. Why deletion requires careful pointer manipulation
Before we remove a node, the program needs to rejoin the list
```cpp
temp->next = nodeToDelete->next;
delete nodeToDelete;
```
After deletion this maintains the connection between the remaining nodes.

### 7. Why dynamically allocated nodes must eventually be de-allocated
This makes use of dynamic memory creates, nodes.
```cpp
Node* newNode = new Node{value, nullptr};
```
And, stops memory leaks as they must eventually be released, through deletion.
```cpp
delete nodeToDelete;
```

### 8. Why a linked list cannot perform binary search efficiently like an indexed array
The middle of a linked list is not directly accessible. And, must follow through each pointer, beginning at the head.
```cpp
Node* temp = head;

while (temp != nullptr) {
  temp = temp->next;

}
```
This is because Binary search is ineffective for a linked list since it cannot directly access an index.

### 9. When a linked list may be preferable to an array or vector
When frequent insertions, and deletions are required a linked list can be helpful. As an example it is effective to insert at the beginning:
```cpp
void insertFront(int value) {
  Node* newNode = new Node{value, nullptr};
  newNode->next = head;
  head = newNode;
}
```
This is an O(1) operation.

### 10. When an array or vector may be preferable to a linked list
An array or vector might be better than a linked list.
This is because the program requires to access the elements fast by index an array or vector that requires a linked list to navigate through nodes instead of only accessing an index.
As an example a linked list is created by using:
```cpp
Node* temp = head;
temp = temp->next;
```
Revealing that the program must follow pointers to reach another node.


