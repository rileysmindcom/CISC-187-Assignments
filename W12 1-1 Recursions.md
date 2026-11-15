# Assignment W12 1-1 Recursions

## Lab Objective
My lab objective is to understand how Recursive functions call themselves, and reach a stopping condition, while using the call stack as the primary goal. Then determine a recursive case, and base case. Looking at the progression of recursive functions towards termination. Implement trace recursive calls, and recognize the return values as the call stack, and understanding the stack overflow, and infinite recursion through comparing recursive, and other steps to these methods. At last writing small recursive functions to recognize any problems with a solution.   

## Assignment W12 1-1 Recursions Code
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

void countdown(int number) {
    for (int i = number; i >= 0; i--) {
        cout << i << endl;
    }
}

void countdownRecursive(int number) {
    cout << number << endl;

    if (number == 0) {
        return;
    }

    countdownRecursive(number - 1);
}

int factorial(int n) {
    if (n == 1) {
        return 1;
    }

    return n * factorial(n - 1);
}

int sumTo(int n) {
    if (n == 0) {
        return 0;
    }

    return n + sumTo(n - 1);
}

void mystery(int n) {
    if (n == 0) {
        return;
    }

    mystery(n - 1);
}

struct Directory {
    string name;
    vector<Directory> subdirectories;
};

void traverseDirectory(Directory directory) {
    cout << directory.name << endl;

    for (Directory subdirectory : directory.subdirectories) {
        traverseDirectory(subdirectory);
    }
}

int main() {

    cout << "Iterative Countdown:" << endl;
    countdown(5);

    cout << "\nRecursive Countdown:" << endl;
    countdownRecursive(5);

    cout << "\nFactorial of 4:" << endl;
    cout << factorial(4) << endl;

    cout << "\nSum from 1 through 5:" << endl;
    cout << sumTo(5) << endl;

    cout << "\nDirectory Traversal:" << endl;

    Directory projects = {
        "projects",
        {
            {
                "cpp",
                {
                    { "graphs",{} },
                    { "recursion",{} }
                }
            },
            {
                "python",
                {
                    {
                        "data",
                        {
                            { "raw",{} },
                            { "processed",{} }
                        }
                    }
                }
            },
            { "notes",{} }
        }
    };

    traverseDirectory(projects);

    return 0;
}
```

## Background Information
We use a programming technique called Recursion to allow the function to call itself to fix the smaller versions of the same problem. A recursion function requires a recursive case that pushes the function to the stopping condition, and base case that will end any repeated calls. A call stack is what the program uses to organize these calls, and ensure that the function is called to the most recent ends before the function comes before it does.
Here, is a visual diagram. We can learn from:


