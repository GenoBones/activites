# Insertion Sort and Algorithm Efficiency

## 1. Average-Case Analysis of Insertion Sort

If we have an array of [5, 2, 8, 4, 7], the first element is already considered sorted. Then the algorithm starts with the second element, which becomes the key. The key is essentially just the value that insertion sort is trying to get to the correct position within the sorted portion. Since 5 > 2, 5 shifts one position to the right and 2 is inserted before it. The array is now [2, 5, 8, 4, 7]. At this point [2, 5] is the sorted portion and 8 becomes the new key. Since 8 > 5 no shift is needed. The next key is 4, which has to be compared against the numbers in the sorted portion. Since 8 > 4, 8 shifts right, and since 5 > 4, 5 shifts right as well. The 4 is then inserted between 2 and 5.

The sorted portion becomes bigger and bigger with every loop. With N elements, the number of elements that may need to be checked grows roughly like 1, 2, 3, 4, and so on up to N - 1. Generally the key will usually have to move through about half of the sorted portion. The work can be represented as:

1/2 + 2/2 + 3/2 + ... + (N - 1)/2. This is equivalent to 1/2(1 + 2 + 3 + ... + N - 1), which grows proportionally to N².

Because Big O ignores constants and lower-order terms, the average-case time complexity becomes O(N²). The reason insertion sort has quadratic growth is that each new key may need to be compared with and shifted past a growing number of elements as the sorted portion gets larger.

## Figure with Array:[5, 2, 8, 4, 7] and Key = 4:

Sorted Portion: [2, 5, 8]

Key: 4

Unsorted Portion: [7]

==========================

Comparison 1: 8 > 4

Result: True

Shift: 8 moves one position to the right

Array: [2, 5, 8, 8, 7]

==========================

Comparison 2: 5 > 4

Result: True

Shift: 5 moves one position to the right

Array: [2, 5, 5, 8, 7]

==========================

Comparison 3: 2 > 4

Result: False

==========================

Final insertion position: 4 is inserted between 2 and 5

Final array: [2, 4, 5, 8, 7]

## 2. Changing the Starting Position of Insertion Sort

A = [5, 4, 3, 2, 1]

### Part A — Start at i = 1

Iterations:  
i = 1: Key is 4. It is compared to 5. Because 5 > 4, 5 is shifted to the right. 4 placed before 5 and array becomes [4, 5, 3, 2, 1]. 1 comparison, 1 shift.

i = 2: The key is 3. Compared to both 5 and 4. Both are > 3. Both shift to the right and array becomes [3, 4, 5, 2, 1]. 2 comparisons 2 shifts.

i = 3: Key is 2. Compared to 5, 4, and 3. All three are > 2. All three values shift right. Array becomes [2, 3, 4, 5, 1]. 3 comparisons, 3 shifts.

i = 4: Key is 1. Compared to 5, 4, 3, and 2. All four values are > 1. All four shift right. Final array become [1, 2, 3, 4, 5]. 4 comparisons, 4 shifts.

Comparisons: 1 + 2 + 3 + 4 = 10  
Shifts: 1 + 2 + 3 + 4 = 10 
Total Operations: 20

### Part B — Start at i = 2

Iterations:  
i = 2: Key is 3. Both 4 and 5 are >  3. Both shift to the right. Array becomes [3, 5, 4, 2, 1]. 2 comparisons, 2 shifts.

i = 3: Key is 2. Compared to 4, 5, and 3. All three are shifted right. Array becomes [2, 3, 5, 4, 1]. 3 comparisons, 3 shifts.

i = 4: Key is 1. Compared to 4, 5, 3, and 2. All four values shift right. Final array becomes [1, 2, 3, 5, 4]. 4 comparisons, 4 shifts.
Comparisons: 2 + 3 + 4 = 9 
Shifts: 2 + 3 + 4 = 9
Total Operations: 18

While starting at i = 2 technically reduces the number of operations, the final array is [1, 2, 3, 5, 4], which is not actually sorted.

### Part C — Start at i = 3

Iterations:  
i = 3: Key is 2. Compared to 3, 4, and 5. All 3 shift right. Array becomes [2, 5, 4, 3, 1]. 3 comparisons, 3 shifts.

i = 4: Key is 1. Compared to 3, 4, 5, and 2. All four shift right. Final array becomes [1, 2, 5, 4, 3]. 4 comparisons, 4 shifts.

Comparisons: 3 + 4 = 7 
Shifts: 3 + 4 = 7
Total Operations: 14 

Again, even though fewer operations were performed, the final result [1, 2, 5, 4, 3] is not actually sorted.

### Part D — Correctness

Insertion sort normally starts at i = 1 because the element at index 0 can automatically be considered sorted by itself. From there, every new key is compared against the already sorted portion of the array and inserted into the correct position. The important component of insertion sort is that everything before the current key of is already sorted. If we start at i = 2, we are telling the algorithm that the first two elements are already sorted. In this example they are [5, 4], so that assumption is wrong. Because of this, starting at i = 2 means the algorithm wont sort as intended. The error gets worse at i = 3, and so on. So while reducing the number of iterations reduces the number of operations, that does not mean the algorithm gets better in terms of accuracy. Starting at i = 1 took 20 operations but we had a sorted array at the end. Compare this with 1=2 at 18 operations and i=3 at 14 operations, but neither produced the desired results. Evidently, doing less operations is only an improvement if we end up with the same desired outcome.


## 3. Improving a Search Algorithm

### Part A — Complexity Analysis

The JS function checks every character in the string no matter when X is found. If X is the first character, foundX gets changed to true , but the loop continues through the rest of the string. Same if X is somewhere in the middle or near the end. There is no exit logic once the value is found. If X is the final character, the algorithm has to check every character before reaching it. If X does not exist at all, it also has to check every character before returning false. Thus, the code always goes through all characters.

Best Case: O(N)  
Average Case: O(N)  
Worst Case: O(N)

### Part B — Improve the Algorithm
The best way to improce the above code is to have a way to exit once the desired character is found:

```javascript
function containsX(string) {
    for (let i = 0; i < string.length; i++) {
        if (string[i] === "X") {
            return true;
        }
    }

    return false;
}
```

### Part C — Analyze the Improved Version

The improved version has a best-case complexity of O(1) because if X happens to be in the first position, it only needs to make one comparison.

For the average case the complexity is still O(N) since X could be somewhere in the middle of the string and the algorithm may need to check some amount of the N characters before finding it. The number of comparisons still grows in relation to N so the average case is thus O(N).

In the worst case we are still looking at a complexity of O(N) since X is either the last character or does not exist at all. In either case, the algorithm still has to check all N characters and the complexity scales linearly.

All in all, the improved version still has a worst case complexity of O(N) just like the original. However, that does not mean the two functions perform exactly the same. The original code always checks every character even when X has already been found, but he improved version can stop early. This potentially reduces the actual number of operations in the best case and many average cases. This shows that two algorithms can have the same worst-case Big O while one can still be more efficient when all is said and done. 


## Analysis and Reflection

Insertion sort has O(N²) behavior in the average and worst cases because each new key may need to be compared to and shifted past a growing number of elements in the sorted portion. Starting at i = 1 is important because the algorithm assumes everything before current key is already sorted. If we start later the assumption could be false. So although fewer operations may occur it does not necessarily mean you will get the desired result.

Reducing the number of operations is not always the same as reducing Big O complexity. As demonstrated above the improved JS function can stop early and ultimately need fewer comparisons. However, it's worst-case complexity is still O(N). This also shows why two algorithms with the same worst case Big O can still perform differently in practice.

Big O tells us how an algorithm scales as N grows, but it does not tell us the exact number of operations that will occur in every case.




