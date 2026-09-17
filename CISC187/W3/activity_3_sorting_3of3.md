AI DISCLAIMER: Chat GPT was used to generate a slightly disordered array. No other use AI code was used.   
QUERY: "can you generate me an array with 50 elements that is slightly disordered. There should be 10 disordered pairs for me to test some sorting algos with"

OUTPUT: "Here’s a 50-element array with exactly 10 adjacent out-of-order pairs. That should work well for testing your function:"
```
#include <iostream>
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
Since a 50 element array has 49 adjacent pairs, and my array is known to have 10 disordered adjacent pairs, that means only about 20% of the adjacent pairs are out of order. Because the large majority of the array is still ordered correctly, I am considering this to be Best/Nearly Sorted. To keep things simple, arrays with about 20% or fewer disordered adjacent pairs will be considered nearly sorted. Anything in the middle will be considered Average/Partially Ordered, while arrays with a large majority of disordered adjacent pairs will be considered Worst/Highly Reverse-Ordered. That being said, the explicit threshold values are as follows:

0–10 OoO pairs = Best/Nearly Sorted
11–34 OoO pairs = Average/Partially Ordered
35–49 OoO pairs = Worst/Highly Reverse-Ordered

```
#include <iostream>
#include <string>
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

string classifyOrderlinessThreshhold(int disorderedPairCount)
    {
        if (disorderedPairCount >= 0 && disorderedPairCount <= 10)
        {
            return "best";
        }
        else if (disorderedPairCount > 10 && disorderedPairCount <= 34)
        {
         return "average";   
        }
        else
        {
         return "worst";   
        }
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
    
    string orderLevel = classifyOrderlinessThreshhold(disorderedPairCount);

    cout << "Out-of-order adjacent pair count: "
     << disorderedPairCount << endl
     << "Orderliness Threshold is: "
     << orderLevel << endl;

    return 0;
}
```

### Step 3 — Select the Sorting Algorithm

## Part B — Case Classification Without Sorting

## Part C — Complexity of the Classification

## Part D — Documentation and Analysis

### Threshold Definition

### Threshold Justification

### Algorithm Selection

### Time Complexity

## Analysis and Reflection
