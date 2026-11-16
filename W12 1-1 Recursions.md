# Assignment W12 1-1 Recursions

## Lab Objective
My lab objective is to understand how recursive functions call themselves, and reach a stopping condition, while using the call stack as the primary goal. Then determine a recursive case, and base case. Looking at the progression of recursive functions towards termination. Implement trace recursive calls, and recognize the return values as the call stack, and understanding the stack overflow, and infinite recursion through comparing recursive, and other steps to these methods. At last writing small recursive functions to recognize any problems with a solution.   

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

<img width="622" height="622" alt="W12 1-1 Recursions" src="https://github.com/user-attachments/assets/c4a1ffb5-781d-4e2a-b0ea-612e1faa5249" />

## Recognizing Recursion
We look at the function
```cpp
void example() {
    example();
}
```
### 1. Why is the function recursive?
Because the calls are within the functions own body, the function is recursive, and it calls example() again every time the program executes.

### 2. Does this function contain a base case?
It does not because the function doesn't have a condition that instructs it when the condition calls itself

### 3. What happens if example() is called?
Example() will keep calling itself if its called, while the stack overflow will eventually result the program running out of available stack memory

### 4. What will happen to the call stack as the function continues to call itself?
Every recursive function to create a new stack frame. This means the call stack keeps expanding until the available stack memory is used up because there isn't any base case

## From Loop to Recursion

### Iterative Countdown Function
The iterative countdown prints numbers from a positive integer down to zero by using a loop.

We see this through the lines of:
```cpp
void countdown(int number) {
    for (int i = number; i >= 0; i--) {
        cout << i << "\n";
    }
}
```

Another example is:
```cpp
countdown(5);
```

And, here is the output
```cpp
5
4
3
2
1
0
```

### Recursive Countdown Function
We now see the recursive function does the same thing without using a loop

```cpp
void countdownRecursive(int number) {
    cout << number << "\n";

    if (number == 0) {
        return;
    }

    countdownRecursive(number - 1);
}
```

As an example:

```cpp
countdownRecursive(5);
```

### Understanding the Recursive Function
#### 1. What is a base case? 
We see Number == 0 is an example of a base case. So the function will return, and stops calling itself when the value drops to zero.

#### 2. What is the recursive case?
The lines of
```cpp
CountdownRecursive(number - 1)
```

Are an example of a recursive case, and calls the function again with a smaller number.

#### 3. How does each call move toward the base case?
The number is reduced by one with each recursive call until it approaches zero.

## Trace the Countdown
We observe the recursive function in this case:
```cpp
void countdown(int number) {
    cout << number << "\n";

    if (number == 0) {
        return;
    }

    countdown(number - 1);
}
```

So we trace 
```cpp
countdown(3)
```

Here, is a chart we can look at:
```
Step                    Function Call            Value Printed            Next Action
1                        countdown(3)                 3                Calls countdown(2)
2                        countdown(2)                 2                Calls countdown(1)
3                        countdown(1)                 1                Calls countdown(0)
4                        countdown(0)                 0              Base case reached; returns
```

### Why does the base case prevent the function from continuing to negative numbers?
Because the base determines when the value is equal to zero. The return statement ends the current function call when the function hits zero. The function never reaches negative integers because no additional recursive call is made.

## Reading Recursive Code Factional
We now look at the function:
```cpp
int factorial(int n) {
    if (n == 1) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

#### 1. What is the base case?
We know the base case is 

```
n == 1
```
So the function will stop performing recursive calls, and return one when it reaches one.


#### 2. What is the recursive case?
The recursive case is 

```
return n * factorial(n - 1);
```

We use a smaller number to call the factorial function multiplied by the current number

#### 3. How does n - 1 move the function toward the base case?
Every recursive call reduces n by one. This is caused because the number drops until it approaches one at which point the base case is triggered.

#### 4. What does factorial(1) return?
```cpp
factorial(1) = 1
```
#### 5. We use these results to determine factorial(2)
```cpp
factorial(2)
= 2 * factorial(1)
= 2 * 1
= 2
```
#### 6. Then we determine the previous result by using factorial(3)
```cpp
factorial(3)
= 3 * factorial(2)
= 3 * 2
= 6
```

#### 7. Determine factorial(4)
```cpp
factorial(4)
= 4 * factorial(3)
= 4 * 6
= 24
```
## Expanding Recursive Calls
Factorial(4) should be expanded until the base is reached
```cpp
factorial(4)
= 4 * factorial(3)
= 4 * 3 * factorial(2)
= 4 * 3 * 2 * factorial(1)
= 4 * 3 * 2 * 1
= 24
```

### When does the period of recursive calls end?
When the factorial(1) is reached, the recursive calling phase ends. The function calls now start to finish when the base case returns one

## The Call Stack
We can trace the call stack for factorial(4)

Here, is an order when these functions are called
```
Call Order        Function
1                factorial(4)
2                factorial(3)
3                factorial(2)
4                factorial(1)
```
And, an order when those functions complete
```
Completion Order        Function
1                      factorial(1)
2                      factorial(2)
3                      factorial(3)
4                      factorial(4)
```

Why is the completed order is the reverse of the call order?
Because the call stack follows the Last In, and First Out(LIFO) order through the completion order is the opposite of the call order. The function that is called last is the first to complete. This is a result before factorial 2 is completed, and factorial 1 must return. And, until factorial(4) is fully finished that same process repeats.

## Passing Values Up the Call Stack
We finish the factorial (4) return value trace

```
Function                Expression            Returned Value
factorial(1)            Returns 1                   1
factorial(2)               2 * 1                    2
factorial(3)               3 * 2                    6
factorial(4)               4 * 6                    24
```

### What does it mean for a value that is being passed back up through the call stack?
When a value is passed back up through the call stack, the preceding function call uses a value that the base case returns to complete the calculation. The Factorial 1. As an example, yields one. Factorial 2, computes two using that result. And, factorial 3 computes six using two compared to factorial 4 will compute twenty four using six. This is until the initial function call is completed, and the process continues on.

## Debugging Recursive Functions
### Example A
We consider the function
```cpp
void mystery(int n) {
    if (n == 0) {
        return;
    }

    mystery(n + 1);
}
```

We assume mystery(5) is called

### 1. Does this function contain a base?
Yes, because the function is instructed to end when the value hits zero. So the base case is n == 0.

### 2. Will the base case ever be reached?
No. because the number continues to rise instead of falling if the function begins with mystery(5). AS a result it will never drop to zero.

### 3. What is wrong with the recursive step?
The recursive step will increase the argument with each call by using 
```
n + 1
```
Instead of brining the function closer to the base case, this will cause it to move farther away.

### 4. How could I correct this?
Instead of adding one, the recursive step could subtract one.
Here's how we could look at this:
```cpp
void mystery(int n) {
    if (n == 0) {
        return;
    }

    mystery(n - 1);
}
```

Now the function slowly can reduce the value from five to zero

### Example: B
We look at

```
void mystery2(int n) {
    cout << n << "\n";
    mystery2(n - 1);
}
```

#### Why is the function eventually going to cause a stack overflow?
Because it will repeat runs without a base case. So the function will eventually result in a stack overflow. And, there is no condition that will instruct the function to stop. Even through the number lowers with each recursive call. It will eventually create more stack frames as it continues into negative values. The program will run out of accessible stack memory as the call stack continues to rise as a result to this.

## Write a Recursive Function
We write a recursive function that will calculate the sum of integers from one through n

```cpp
sumTo(5)
```

This gives us a result of:
```
5 + 4 + 3 + 2 + 1 = 15
```

### Base Case

Its when n equals zero, which is a base case. Because there are no more numbers to add, the function returns zero.

### Recursive Case
The recursive case will call the algorithm again with n - 1, and add the current number to the result.

### How the recursive argument moves toward the base case?
Every recursive call reduces n by one. When the value drops to zero. The recursion is terminated as the base case occurs.

Recursion vs. Iteration
Compare a loop-based countdown with a recursive countdown.

### Questions, and Responses
#### 1. Which version is easier for you to read?
We know the loop makes it obvious how the numbers drop until they hit zero, I find the iterative form easier for reading.
#### 2. Which version uses the call stack? 	
The function call will create a new stack frame, and the recursive version will make make a call stack as an advantage.
#### 3. Is recursion automatically more efficient than iteration? 	
No because the recursion uses more stack memory, and creates more function call, and isn't necessary efficient
#### 4. Why might a loop still be preferable for a simple countdown?
For a basic countdown a loop would be preferable because it is simple, and would require less memory to eliminate any calls to the recursive functions.

## Recursive Structure: Filesystem Traversal
We look at the nested directory structure
```
projects/
    cpp/
        graphs/
        recursion/
    python/
        data/
            raw/
            processed/
    notes/
```
#### 1. Why is recursion a natural approach to this problem?
We look at the directories contain other directories which can contain more directories. This makes sense for recursion. After the process to the current directory, a recursive function is called to examine every sub-directory.

#### 2. What could serve as the base case when processing a directory?
When we read the directory with no more sub-directories could be a base scenario. Meaning the function would return when there are no more sub-directories to look at.

#### 3. What would the recursive case do?
The recursive case would call the same function to explore each sub-directory. As a result the directories at various nesting levels can continue to be visited by each process.

#### 4. Why is it difficult to know in advance how many levels of nesting may exist?
We understand different directories can have a varying amounts of sub-directories, and nesting levels but it can be a challenge to determine this. When some directories may have multiple levels others may have no sub-directories at all.

Here, is Pseudo-code to read the directory traversal. To understand this process

### Pseudocode to Recursive Directory Traversal
```
function traverseDirectory(directory):

    process the current directory

    for each subdirectory inside directory:

        traverseDirectory(subdirectory)

    return
```

And, another way we can use this through C++

```
void traverseDirectory(Directory directory) {

    cout << directory.name << endl;

    for (Directory subdirectory : directory.subdirectories) {

        traverseDirectory(subdirectory);

    }
}
```

These examples help us understand the directory functions unique to the operating system as they are designed in a recursive structure. So after processing the current directory. The program traverses each sub-directory recursively. The loop will end, and the method goes back to the previous call when a directory has no more sub-directories.

## Analysis, and Reflection
#### 1. Why every useful recursive function needs a stopping condition.
The stopping condition was necessary for a meaningful recursive function to understand when the stop calling occurs. So the code will run endlessly without a base case, which would eventually result into a stack overflow.

#### 2. Why simply having a base-case condition is not enough if the recursive step never reaches it.
If the recursive step deviates from a base case. Then having a base case would be insufficient. As each recursive call, the function must move closer to the stopping condition.

#### 3. What happens during the recursive calling phase.
The function will repeat any calls with altered parameters throughout the recursive calling phase. Meaning calls will create a new stack frame, and will contain details about the incomplete function.

#### 4. What happens during the unwinding phase.
Once the base case is reached the recursive calls will start offering their results during the unwinding phase. Until the initial call is completed. Each function call uses the returned value to continue the calculation.

#### 5. Why the call stack behaves naturally as a LIFO structure for recursion.
Because the Last In, and Firs tout principle controls the call stack. Before the previous function call can proceed, The most recent function call must be completed. As a result any incomplete recursive operations can be observed.

#### 6. Why infinite recursion can cause stack overflow.
Because without letting the preceding calls complete, infinite recursion generates an increasing number of function calls. the more stack memory is used for each call. And, a stack overflow will occur when the program runs out of stack space.

#### 7. Why recursion is not automatically preferable to iteration.
This is because it can increase memory usage so a function call overhead, recursion wouldn't always be an ideal option. As an example activities such as counting or repeating an action, iteration is easier instead of repeating the operation.

#### 8. Why recursion is useful for problems containing an unknown number of nested levels.
Recursion can be helpful because it allows the function to repeat the process deeper through a smaller portion of the structure without requiring any prior knowledge to the number of levels. I find it useful for problems such as traversing directories, and other nested patterns

## Challenges
Some of the challenges were understanding how recursive functions call themselves, and how the base case stops them, which was difficult but made it easier at the end of this experiment. This is because I had to trace each function call, and understand how the returned values are used through each of the calculations, and tracing the factorial function. This gave myself a chance to learn the call stack, and the operations used, and why the functions will terminate the opposite order. I found this easier when working through the recursive examples, which improved my understanding of the recursion concept, and has taught myself to prevent any issues such as stack overflow, and infinite recursion.

## Assignment Video Link:
https://drive.google.com/file/d/15cmcR5560XIRS3f6BuSTtno5TvazHpM7/view?usp=sharing









