# Recursion Lab

## Part 1 — Recognizing Recursion

1.) Because the function calls itself within the body of the function  

2.) No  

3.) The function will continue forever because there is no way to exit.   

4.) The call stack continues to grow until there is no more space and it causes stack overflow.  


## Part 2 — From Loop to Recursion

### Iterative  
```
#include <iostream>

using namespace std;

void countdown(int num)
{
    while (num>=0){
        
        cout << num << endl;
        
        num --;
    }
        
        
}

int main()
{
    countdown(3);

    return 0;
}
```

### Recursion  
```
#include <iostream>

using namespace std;

void countdown(int num)
{
    cout << num << endl;

    if (num == 0)
    {
        return;
    }

    countdown(num - 1);
}

int main()
{
    countdown(5);

    return 0;
}
```


1.) currentNum < 0  

2.) countdown(currentNum)  

3.) currentNum decreases by 1 before each recursive call.  


## Part 3 — Trace the Countdown

### Countdown Trace  
```
| Step | Function Call  | Value Printed | Next Action               |
|-----:|----------------|--------------:|---------------------------|
| 1    | countdown(3) | 3             | Call countdown(2)           |
| 2    | countdown(2) | 2             | Call countdown(1)           |
| 3    | countdown(1) | 1             | Call countdown(0)           |
| 4    | countdown(0) | 0             | Base case reached — return  |
```
1.) Since the function returns before calling countdown(number - 1) again, it never calls countdown(-1).

## Part 4 — Reading Recursive Code: Factorial

1.) n == 1

2.) return n * factorial(n - 1);

3.) n - 1 decreases the value of n by 1 with each call until it reaches the base case of 1.

4.) 1

5.) 2

6.) 6

7.) 24


## Part 5 — Expanding Recursive Calls

### factorial(4) Expansion

```
factorial(4)
= 4 * factorial(3)
= 4 * 3 * factorial(2)
= 4 * 3 * 2 * factorial(1)
= 4 * 3 * 2 * 1
= 24
```
Each call gets expanded until factorial(1) reaches the base case and returns 1

## Part 6 — The Call Stack

### Call Order  
1: factorial(4)  

2: factorial(3)  

3: factorial(2)  

4: factorial(1)  

### Completion Order

1: factorial(1)

2: factorial(2)  

3: factorial(3)  

4: factorial(4)  

1.) It is reversed because the call stack uses LIFO. Each function has to wait for the recursive function it called to finish before it can complete.

## Part 7 — Passing Values Up the Call Stack

```
factorial(1): | Expression: 1     | Returned Value: 1  
factorial(2): | Expression: 2 * 1 | Returned Value: 2  
factorial(3): | Expression: 3 * 2 | Returned Value: 6  
factorial(4): | Expression: 4 * 6 | Returned Value: 24  
```


## Part 8 — Debugging Recursive Functions

### Example A

1.) Yes: n == 0.

2.) No: The value keeps increasing, so it moves farther away from 0.

3.) It uses n + 1 instead of n - 1.

4.) Change n + 1 to n - 1.

### Example B

1.) Stack overflow is caused because it has no base case and the function is called forever. 


## Part 9 — Write a Recursive Function

1.) Base case: n == 1, return 1.  

2.) Recursive case: return n + sumTo(n - 1);  

3.) It decreases by 1 each call until it reaches the base case of 1.  

```
#include <iostream>

using namespace std;

int sumTo(int n)
{
    if (n == 1)
    {
        return 1;
    }

    return n + sumTo(n - 1);
}

int main()
{
    cout << sumTo(5) << endl;

    return 0;
}
``` 
### sumTo(4) Trace  

Calls:  
sumTo(4)  
sumTo(3)   
sumTo(2)  
sumTo(1)  

Returned values:  
sumTo(1): | Expression: 1     | Returned Value: 1  
sumTo(2): | Expression: 2 + 1 | Returned Value: 3  
sumTo(3): | Expression: 3 + 3 | Returned Value: 6   
sumTo(4): | Expression: 4 + 6 | Returned Value: 10  
  
## Part 10 — Recursion vs. Iteration

1.) By far, the loop-based version is easier for me to read because I can see the value changing step by step without having to keep track of multiple function calls.

2.) The recursive version.

3.) No. Recursion is not automatically more efficient than iteration and can use more memory as the call stack grows.

4.) A loop may be preferable for a simple countdown because it is easier to follow and does not require multiple function calls or extra call-stack space.


## Part 11 — Recursive Structure: Filesystem Traversal

1.) Because each directory can contain more directories, and the same process can be repeated for each one no matter how deep the nesting is.

2.) Perhaps reaching a directory that contains no more subdirectories.

3.) Ideally it would go through each subdirectory and call the same function again for that directory.

4.) Because directories can be nested to different depths and without looking at a diagram or running a function, there is no immediate way to know.

### Recursive Filesystem Traversal
```
traverseDirectory(directory)

    visit the current directory

    if the directory has no subdirectories
        return

    for each subdirectory
        traverseDirectory(subdirectory)
```

## Analysis and Reflection

1.) A recursive function needs a stopping condition so it does not continue calling itself forever.

2.) A base case only works if the recursion actually moves toward it. 

3.) Each function call is added to the call stack and waits for the next recursive call to finish.

4.) The base case returns first, and then each waiting function call finishes in reverse order.

5.) Because the most recently called function has to finish before the earlier function can continue.

6.) Infinite recursion keeps adding new function calls to the call stack until the stack runs out of space.

7.) Recursion is not always better than iteration because it can use more memory and may be harder to follow for simple problems.

8.) Because the same function can keep calling itself until it reaches the end of the structure.
