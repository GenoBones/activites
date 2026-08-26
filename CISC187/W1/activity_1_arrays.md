# Array Data Structure

## Task 1 - Creating an Array

```cpp
#include <iostream>
using namespace std;

int main() {
    int numArray[100];

    cout << sizeof(numArray[0]) << endl;

    return 0;
}
```

## Task 2 - Size of Each Element

Each element is 4 bytes.

## Task 3 - Array Operations

### 3a - Reading an Element

It only takes 1 step to read an array element.

### 3b - Searching for an Element That Is Not in the Array

It will take 100 steps to search for something that is not in the array.

### 3c - Inserting at the Beginning

Every element in the array would need to be shifted in order to make room to insert a new value at the beginning, so there would be 100 shifts given that there are 100 existing values. Then the new value would need to be inserted, which would be another step. The final step count would therefore be 101.

### 3d - Inserting at the End

This would only take 1 step, as nothing else needs to be moved.

### 3e - Deleting from the Beginning

It is my understanding that deleting at the beginning of an array means that every other value would need to shift down an index. Therefore, after the value is deleted, there are 99 remaining values that need to shift. If the deletion itself is considered a step, then the total would be 100 steps.

### 3f - Deleting from the End

Nothing needs to shift here, so this would just be 1 step.

## Task 4 - Finding Every Instance

Every element in the array would need to be checked because the search cannot stop after finding the first occurrence. Therefore, for an array containing N elements, N comparisons would be required. In Big O notation, this would be O(N).

## Task 5 - Memory Address
```cpp
#include <iostream>
using namespace std;

int main()
{
    int numbers[100];

    cout << sizeof(numArray[0]) << endl;
    cout << numArray << endl;

    return 0;
}
```
