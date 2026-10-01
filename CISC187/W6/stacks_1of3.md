# Implementing and Applying Stacks in C++

## Part 1 — Trace Stack Operations

### Stack Trace  
push(10): Value Returned: NA | Stack: [10] | Top: 10 | Size: 1
push(20): Value Returned: NA | Stack: [10, 20] | Top: 20 | Size: 2
push(30): Value Returned: NA | Stack: [10, 20, 30] | Top: 30 | Size: 3
pop(): Value Returned: 30 | Stack: [10, 20] | Top: 20 | Size: 2
push(40): Value Returned: NA | Stack: [10, 20, 40] | Top: 40 | Size: 3
push(50): Value Returned: NA | Stack: [10, 20, 40, 50] | Top: 50 | Size: 4
pop(): Value Returned: 50 | Stack: [10, 20, 40] | Top: 40 | Size: 3
push(60): Value Returned: NA | Stack: [10, 20, 40, 60] | Top: 60 | Size: 4 

### Analysis
The final top element is 60. The final stack size is 4. The remaining elements would be removed in the order 60, 40, 20, 10.
That demonstrates LIFO because the most recently added value is always the first one removed.

## Part 2 — Implement an Array-Based Stack

Simple copy and paste the structure from the assignment in preparation for part 3.


## Part 3 — Maintaining topIndex  

```
#include <iostream>
#include <stdexcept>
using namespace std;

class Stack
{
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

Stack::Stack()
{
    topIndex = -1;
}

bool Stack::empty() const
{
    return topIndex == -1;
}

bool Stack::full() const
{
    return topIndex == CAPACITY - 1;
}

int Stack::size() const
{
    return topIndex + 1;
}

int main()
{
    Stack stack;
    
    cout << boolalpha;
    cout << "Stack empty: " << stack.empty() << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;

    return 0;
}
```
### Analysis  
1: Because index 0 represents the first element in the stack. A topIndex of -1 means the stack is empty.
2: Because arrays use zero-based indexing. If topIndex is 0, there is 1 element, if it is 3, there are 4 elements.
3: CAPACITY - 1. With a capacity of 10, the stack is full when topIndex is 9.

## Part 4 — Implement push()  
```
#include <iostream>
#include <stdexcept>
using namespace std;

class Stack
{
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

Stack::Stack()
{
    topIndex = -1;
}

bool Stack::empty() const
{
    return topIndex == -1;
}

bool Stack::full() const
{
    return topIndex == CAPACITY - 1;
}

int Stack::size() const
{
    return topIndex + 1;
}

void Stack::push(int value)
{
    if (full())
    {
        cout << "Stack is full. Throwing overflow error." << endl; //c++ shell not throwing error for some reason
        return;
        //throw overflow_error("Stack overflow");
    }

    topIndex++;
    data[topIndex] = value;
}

int main()
{
    Stack stack;

    cout << boolalpha;

    cout << "Stack empty: " << stack.empty() << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;

    cout << endl;

    stack.push(10);
    stack.push(20);
    stack.push(30);

    cout << "After pushing 10, 20, and 30:" << endl;
    cout << "Stack empty: " << stack.empty() << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;

    cout << endl;
    
    stack.push(40);
        stack.push(50);
        stack.push(60);
        stack.push(70);
        stack.push(80);
        stack.push(90);
        stack.push(100);
        
    cout << "After filling array:" << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;
    
    cout << endl;
    
    cout << "Now attempting extra push" << endl;
    
    stack.push(110);
    
    return 0;
}
```
### Analysis  
Writing beyond data[CAPACITY - 1] is invalid because that is the last available index in the array. Trying to push another value after index 9 would go outside of the array's allocated memory.


## Part 5 — Implement pop()  
```
#include <iostream>
#include <stdexcept>
using namespace std;

class Stack
{
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

Stack::Stack()
{
    topIndex = -1;
}

bool Stack::empty() const
{
    return topIndex == -1;
}

bool Stack::full() const
{
    return topIndex == CAPACITY - 1;
}

int Stack::size() const
{
    return topIndex + 1;
}

void Stack::push(int value)
{
    if (full())
    {
        cout << "Error: Stack overflow." << endl; //c++ shell not throwing error for some reason
        return;
        //throw overflow_error("Stack overflow");
    }

    topIndex++;
    data[topIndex] = value;
}

int Stack::pop()
{
    if (empty())
    {
        cout << "Error: Stack underflow" << endl;
        return -1;
        //throw underflow_error("Stack underflow");
    }

    int value = data[topIndex];
    topIndex--;
    return value;
}

int main()
{
    Stack stack;

    cout << boolalpha;

    cout << "Stack empty: " << stack.empty() << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;

    cout << endl;

    stack.push(10);
    stack.push(20);
    stack.push(30);

    cout << "After pushing 10, 20, and 30:" << endl;
    cout << "Stack empty: " << stack.empty() << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;

    cout << endl;
    
    stack.push(40);
    stack.push(50);
    stack.push(60);
    stack.push(70);
    stack.push(80);
    stack.push(90);
    stack.push(100);
        
    cout << "After filling array:" << endl;
    cout << "Stack full: " << stack.full() << endl;
    cout << "Stack size: " << stack.size() << endl;
    
    cout << endl;
    
    cout << "Now attempting extra push" << endl;
    
    stack.push(110);

    cout << endl;

    cout << "Removing values from stack:" << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "Popped: " << stack.pop() << endl;

    cout << endl;

    cout << "Stack empty: " << stack.empty() << endl;
    cout << "Stack size: " << stack.size() << endl;

    cout << endl;

    cout << "Now attempting extra pop" << endl;
    stack.pop();

    return 0;
}
```

### Analysis


## Part 6 — Implement top()

### top() Implementation

### Analysis


## Part 7 — Test the Complete Stack

### Complete Program

### Test Results


## Part 8 — Complexity Analysis

### push()

### pop()

### top()

### empty()

### full()

### size()

### One Million Elements


## Part 9 — Stack Correctness

### Operation Results

### Analysis


## Part 10 — Balanced Delimiters

### Algorithm

### Analysis


## Part 11 — Implement the Balanced-Delimiter Algorithm

### C++ Implementation

### Test Results

### Important Cases


## Part 12 — Analyze Delimiter Matching

### Time Complexity

### Space Complexity

### Analysis


## Part 13 — Stack Applications

### Scenario A — Undo

### Scenario B — Function Calls

### Scenario C — Browser Back Navigation

### Scenario D — Depth-First Search

### Scenario E — Customer Service Line


## Analysis and Reflection
