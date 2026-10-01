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
A topIndex of -1 means the stack is empty. Since -1 is not a valid array index, trying to access data[-1] would attempt to read memory outside of the array

## Part 6 — Implement top()
#include <iostream>
#include <stdexcept>
using namespace std;
```
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

int Stack::pop()
{
    if (empty())
    {
        cout << "Stack underflow" << endl;
        return -1;
        //throw underflow_error("Stack underflow");
    }

    int value = data[topIndex];
    topIndex--;
    return value;
}

int Stack::top() const
{
    if (empty())
    {
        cout << "Stack underflow" << endl;
        return -1;
        //throw underflow_error("Stack underflow");
    }

    return data[topIndex];
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
    cout << "Top value: " << stack.top() << endl;

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
    cout << "Top value: " << stack.top() << endl;

    cout << endl;

    cout << "Now attempting extra push" << endl;

    stack.push(110);

    cout << endl;

    cout << "Removing values from stack:" << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "New top: " << stack.top() << endl;
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

    cout << endl;

    cout << "Now attempting top on empty stack" << endl;
    stack.top();

    return 0;
}
```
### Analysis
top() returns the current top element without changing the stack. pop() returns the top element and removes it..

## Part 7 — Test the Complete Stack

### Complete Program
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

int Stack::pop()
{
    if (empty())
    {
        cout << "Stack underflow" << endl;
        return -1;
        //throw underflow_error("Stack underflow");
    }

    int value = data[topIndex];
    topIndex--;
    return value;
}

int Stack::top() const
{
    if (empty())
    {
        cout << "Stack underflow" << endl;
        return -1;
        //throw underflow_error("Stack underflow");
    }

    return data[topIndex];
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
    cout << "Top value: " << stack.top() << endl;

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
    cout << "Top value: " << stack.top() << endl;

    cout << endl;

    cout << "Now attempting extra push" << endl;

    stack.push(110);

    cout << endl;

    cout << "Removing values from stack:" << endl;
    cout << "Popped: " << stack.pop() << endl;
    cout << "New top: " << stack.top() << endl;
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

    cout << endl;

    cout << "Now attempting top on empty stack" << endl;
    stack.top();

    return 0;
}
```

### Test Results
Stack empty: true  
Stack full: false  
Stack size: 0  

After pushing 10, 20, and 30:  
Stack empty: false  
Stack full: false  
Stack size: 3  
Top value: 30  

After filling array:  
Stack full: true  
Stack size: 10  
Top value: 100  

Now attempting extra push  
Stack is full. Throwing overflow error.  

Removing values from stack:  
Popped: 100  
New top: 90  
Popped: 90  
Popped: 80  
Popped: 70  
Popped: 60  
Popped: 50  
Popped: 40  
Popped: 30  
Popped: 20  
Popped: 10  

Stack empty: true  
Stack size: 0  

Now attempting extra pop  
Stack underflow  

Now attempting top on empty stack  
Stack underflow  

## Part 8 — Complexity Analysis  
push(): O(1) - It checks if the stack is full, increments topIndex, and stores one value.
pop(): O(1) - It checks if the stack is empty, accesses the top value, and decreases topIndex.
top(): O(1) - It directly accesses data[topIndex] without searching the array.
empty(): O(1) - It only checks whether topIndex is equal to -1.
full(): O(1) - It only checks whether topIndex is equal to CAPACITY - 1.
size(): O(1) - It calculates the size using topIndex + 1.

### One Million Elements  
No. The stack always keeps track of the top element using topIndex, so pop() can directly access the top value and decrease topIndex. This is why it remain O(1) no matter the number of elements. 

## Part 9 — Stack Correctness  
The values will be returned 20, 15, 10, and 5. 

Typical rule of thumb as outlined in the lecture - If the values are pushed in the order of: x1, x2, x3, ... xN,   
The pops will return: xN, xN-1, ... x2, x1

values should come out in the reverse order that they were added. If they do, the stack is correctly following the LIFO.


## Part 10 — Balanced Delimiters  
Good to know. 


## Part 11 — Implement the Balanced-Delimiter Algorithm

### C++ Implementation  
```
#include <iostream>
#include <stdexcept>
#include <stack>
#include <string>
using namespace std;

bool balanced(const string& expression)
{
    stack<char> delimiters;

    for (char currentCharacter : expression)
    {
        if (currentCharacter == '(' ||
            currentCharacter == '[' ||
            currentCharacter == '{')
        {
            delimiters.push(currentCharacter);
        }

        else if (currentCharacter == ')' ||
                 currentCharacter == ']' ||
                 currentCharacter == '}')
        {
            if (delimiters.empty())
            {
                return false;
            }

            char openingDelimiter = delimiters.top();

            if ((currentCharacter == ')' && openingDelimiter != '(') ||
                (currentCharacter == ']' && openingDelimiter != '[') ||
                (currentCharacter == '}' && openingDelimiter != '{'))
            {
                return false;
            }

            delimiters.pop();
        }
    }

    return delimiters.empty();
}

int main()
{

    cout << "Balanced delimiter tests:" << endl;

    cout << "{(a+b)*[c-d]}: "
         << balanced("{(a+b)*[c-d]}") << endl;

    cout << "((a+b)): "
         << balanced("((a+b))") << endl;

    cout << "((a+b): "
         << balanced("((a+b)") << endl;

    cout << "(a+b]: "
         << balanced("(a+b]") << endl;

    cout << "[(())]: "
         << balanced("[(())]") << endl;

    cout << "{[()]}: "
         << balanced("{[()]}") << endl;

    return 0;
}
```

### Test Results  
Balanced delimiter tests:  
{(a+b)*[c-d]}: 1  
((a+b)): 1  
((a+b): 0  
(a+b]: 0  
[(())]: 1  
{[()]}: 1  


## Part 12 — Analyze Delimiter Matching  
  
The algorithm is efficient because it does not repeatedly search through the expression. It processes the characters in order and uses the stack to keep track of unmatched opening delimiters. Because each character is handled only a constant number of times, the total amount of work grows linearly with the length of the expression.


## Part 13 — Stack Applications
A: Most recent action should be undone first. Evidence of LIFO. Stack is appropriate.  
B: Most recently called function must finish before returning to the previous function. Evidence of LIFO. Stack is appropriate. 
C: Most recently visited page is the first page you return to. Evidence of LIFO. Stack is appropriate. 
D: Most recently discovered node or path is explored before earlier alternatives. Evidence of LIFO. Stack is appropriate. 
E: Customers should be served in the order they arrived. Evidence of LIFO. Stack is appropriate. 


## Analysis and Reflection  
1: A stack is an ADT because it is defined by LIFO behavior, not by how it is stored.
2: Access is limited to the top to enforce LIFO order.
3: topIndex gives direct access to the top element, making stack operations O(1).
4: Overflow happens when pushing to a full stack. Underflow happens when removing from an empty stack.
5: An array-based stack has fixed capacity because the array size is fixed when created.
6: Stacks work well for these tasks because the most recent item needs to be handled first.
7: A queue is far better suited for this. Stacks can only access the most recent "arrival".
