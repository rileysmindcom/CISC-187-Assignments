# Assignment W6 > 1/2 Stacks
## Lab Objective
The objective of this lab is to create a stack using Last-in, and First-Out (LIFO) to solve real world problems. To describe the operation of LIFO behavior. Use an array for implementing a stack. Put push(), pop(), top(), full(), and size() as practice. Then analyze the topIndex of the stack. Identifying the underflow, and overflow, trace stack operations. Examine the stack operations complexity. And, then finding out if an expressions delimiters are unbalanced using a stack. Then determining the circumstance, where a stack is suitable or unsuitable.
## Assignment Code
```cpp
#include <iostream>
#include <stdexcept>
#include <string>
#include <stack>

using namespace std;

class Stack {
private:
    static const int CAPACITY = 10;

    int data[CAPACITY];
    int topIndex;

public:
    Stack();

    bool empty() const;
    bool full() const;
    int size() const;

    void push(int value);
    int pop();
    int top() const;
};

Stack::Stack() {
    topIndex = -1;
}

bool Stack::empty() const {
    return topIndex == -1;
}

bool Stack::full() const {
    return topIndex == CAPACITY - 1;
}

int Stack::size() const {
    return topIndex + 1;
}

void Stack::push(int value) {
    if (full()) {
        throw overflow_error("Stack overflow");
    }

    data[++topIndex] = value;
}

int Stack::pop() {
    if (empty()) {
        throw underflow_error("Stack underflow");
    }

    int value = data[topIndex];
    --topIndex;
    return value;
}

int Stack::top() const {
    if (empty()) {
        throw underflow_error("Stack underflow");
    }

    return data[topIndex];
}

bool balanced(const string& expression) {
    stack<char> delimiters;

    for (char ch : expression) {
        if (ch == '(' || ch == '[' || ch == '{') {
            delimiters.push(ch);
        }
        else if (ch == ')' || ch == ']' || ch == '}') {
            if (delimiters.empty()) {
                return false;
            }

            char opening = delimiters.top();

            if ((ch == ')' && opening != '(') ||
                (ch == ']' && opening != '[') ||
                (ch == '}' && opening != '{')) {
                return false;
            }

            delimiters.pop();
        }
    }

    return delimiters.empty();
}

int main() {
    Stack s;

    cout << boolalpha;

    cout << "Initially empty: " << s.empty() << endl;

    s.push(10);
    s.push(20);
    s.push(30);
    s.push(40);
    s.push(50);

    cout << "Size after five pushes: " << s.size() << endl;
    cout << "Top after five pushes: " << s.top() << endl;

    cout << "Popped: " << s.pop() << endl;
    cout << "Popped: " << s.pop() << endl;

    cout << "New top: " << s.top() << endl;
    cout << "New size: " << s.size() << endl;

    cout << "Removing remaining values: ";

    while (!s.empty()) {
        cout << s.pop() << " ";
    }

    cout << endl;
    cout << "Stack empty: " << s.empty() << endl;

    try {
        s.pop();
    }
    catch (const underflow_error& e) {
        cout << "Underflow detected: " << e.what() << endl;
    }

    for (int i = 1; i <= 10; ++i) {
        s.push(i * 10);
    }

    cout << "Stack filled to capacity. Size: " << s.size() << endl;

    try {
        s.push(110);
    }
    catch (const overflow_error& e) {
        cout << "Overflow detected: " << e.what() << endl;
    }

    string expressions[] = {
        "{(a+b)*[c-d]}",
        "{(a+b]*c}",
        "((a+b))",
        "((a+b)",
        "[a+b]",
        "{[()]}",
        "{[(])}"
    };

    cout << endl;
    cout << "Balanced Delimiter Tests" << endl;

    for (const string& expression : expressions) {
        cout << "Expression: " << expression << endl;
        cout << "Balanced: " << balanced(expression) << endl;
    }

    return 0;
}
```
## Analysis, and Reflection

## Flowchart
### Example 1: Trace Stack Operations
The stack begins empty

If the final top element is 60

And, Final stack size is 4
The remaining elements are removed from this order

60, 40, 20, 10

To explain LIFO because the recent piece is usually the first element removed.

### Example 2: Maintaining topIndex
Why should topIndex start with -1

This is because there are no elements in the stack. So topIndex must start out at -1. Then the array at index 0, the first element is kept.

The program can make an incorrect syntax if an element already exists at index 0 if topIndex begins at 0

We can label:

```
topIndex = -1 → 0 elements
topIndex = 0  → 1 element
topIndex = 1  → 2 elements
```
To represent why topIndex + 1 is the stack size

Since indexes in array begin at 0. If the topIndex is the highest occurring index then there are more than one element than that specific index.

As an example
Let's say:
```
topIndex = 0 → size = 1
topIndex = 4 → size = 5
topIndex = 9 → size = 10
```
The topIndex value means that the stack is full?
So the final valid array index shows a capacity of 10, meaning:

CAPACITY - 1 = 9

When the stack is full

topIndex = 9

### Example: 3 push(), and stack Overflow

First the push() explains when the stack is full. A new value is stored at topIndex, which is raised if it isn't full

We can use:

```
value = data[++topIndex];
```
To write beyond

```
data[CAPACITY - 1]
```
Is inaccurate because there is only indexes 0 to 9 in the array. So when writing index 10 can result in undefined behavior because it would access memory outside the array.


However, before adding a new value the application checks full() to prevent this.

The program shows this when the stack is full:
```
throw overflow_error ("Stack overflow");
```
Because the attempt is created to add an element to a stack that has no available capacity. This is considered as stack overflow.

### Example 4: pop(), and Stack Underflow
The pop() function determines if the stack is empty
```
Throw underflow_error ("Stack underflow") if (empty());
```
When the top value is saved the topIndex is lowered, and the saved value returns if there is an element in the stack

We can access

data[topIndex]

Because -1 is not a valid array index, and topIndex = -1 is invalid. So the array's valid indexes are 0 through 9.

So instead of accessing faulty memory trying to pop from an empty stack would result in a stack underflow error.

### Example 5: top() vs. pop()
Both of each operation the element at the top of the stack is accessed by both actions , but their outcomes are different

Because the top element is returned by top() without being removed. Pop() decreases since topIndex removes the top element, and returns it.

Using an example, if the stack is:
```
Top
 ↓
[30]
[20]
[10]
```
Then calls:
```
top()
```
To return 30, leaving the stack unaffected
We re-call:
```
pop()
```
As 30 returns, and modifies the stack to:
```
Top
 ↓
[20]
[10]
```
### Example 6: Complete Stack Testing



# Challenges

# Assignment Video Link
