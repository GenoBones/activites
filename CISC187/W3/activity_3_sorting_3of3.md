AI DISCLAIMER: Chat GPT was used to generate a slightly disordered array. No other use AI code was used.   
QUERY: "can you generate me an array with 50 elements that is slightly disordered. There should be 10 disordered pairs for me to test some sorting algos with"

OUTPUT:
```
int numbers[arraySize] =
{
    1, 2, 4, 3, 5,
    6, 8, 7, 9, 10,
    12, 11, 13, 15, 14,
    16, 17, 19, 18, 20,
    21, 23, 22, 24, 25,
    27, 26, 28, 30, 29,
    31, 32, 34, 33, 35,
    36, 38, 37, 39, 40,
    41, 42, 43, 44, 45,
    46, 47, 48, 49, 50
};
```

# Adaptive Sorting Strategy

## Part A — Adaptive Sorting Selection

### Step 1 — Analyze the Input Order

```#include <iostream>
using namespace std;

int examineOrderliness(const int numbers[], int size)
{
    int disorderedPairCount = 0;

    for (int i = 0; i < size - 1; i++)
    {
        if (numbers[i] > numbers[i + 1])
        {
            disorderedPairCount++;
        }
    }

    return disorderedPairCount;
}

int main()
{
    const int arraySize = 50;
    
    int numbers[arraySize] =
{
    1, 2, 4, 3, 5,
    6, 8, 7, 9, 10,
    12, 11, 13, 15, 14,
    16, 17, 19, 18, 20,
    21, 23, 22, 24, 25,
    27, 26, 28, 30, 29,
    31, 32, 34, 33, 35,
    36, 38, 37, 39, 40,
    41, 42, 43, 44, 45,
    46, 47, 48, 49, 50
};
    int disorderedPairCount = examineOrderliness(numbers, arraySize);

    cout << "Out-of-order adjacent pairs: "
         << disorderedPairCount << endl;

    return 0;
}
```

### Step 2 — Define a Threshold

### Step 3 — Select the Sorting Algorithm

## Part B — Case Classification Without Sorting

## Part C — Complexity of the Classification

## Part D — Documentation and Analysis

### Threshold Definition

### Threshold Justification

### Algorithm Selection

### Time Complexity

## Analysis and Reflection
