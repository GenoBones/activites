# Sorting Algorithms and Big-O Analysis

## 1. Linear Complexity

While the multiplier (in this case 4) changes the number of operations, what bit O notation truly demonstrates is not the number of operations, but how the number of operations grows as "N" increases. Therefor the +16 constant is ignored because as N grows, that 16 becomes less and less significant. So essentially, the complexity of work done on 10 elements vs 10,000 is roughly the same. Because of this, the consatnt multiplier and constant term are ignored. Thus 4N + 16 -> O(N).


## 2. Quadratic Complexity

As stated above, the constant is ignored by big O as it doesnt change the growth complexity. However, the N^2 is relevant because as N increases the amount of work grows significantly faster than it would for a linear algorithm. As an example, if N doubles, O(N^2) becomes 4 timens as large. Doubling the elements from 10 to 20 increases the operation count from 200 to 800. Thus 2N^2 -> O(N^2).


## 3. Analyzing Multiple Sequential Loops

The first loop only goes through the array once. If the array contains N elements, the first loop executes N times. The second loop goes through the doubled array which also has N elements therefor the second loop also executes N times. All in all, the total amount of loop work is about N + N = 2N, but as mentioned above, the constant is ignored and we are left with 2N -> O(N) as the final time complexity. Because the loops run one after another they are not O(N^2). The work that the loop does is added, not doubled. 


## 4. Multiple Constant-Time Operations

Each string in the array is executed once in the loop. So for N strings, it executes N times. This is a linear relationship.

For each loop, three operations are performed: upcase, downcase, and capitalize. Since each one is treated as its own operation, the work put in is 3N, but because big O ignores the constant, we are left with O(N) as the final time complexity. Adding more constants to the operation doesnt affect the growth pattern so the linear relationship remains. 


## 5. Analyzing Nested Iteration

The outer loop runs once for every element in the array N times. index.even is true for about half of the elements which means the inner loop runs about N/2 times. Each time the inner loop is reached, it goes through the entire array N times. So the approximate number of inner-loop operations is (N / 2) * N, which just becomes N^2 / 2 and then big O ignores the 1/2 leaving the final time complexity as O(N^2)

Only processing every other element of the outer loop does reduce the exact number of operations, but the quadratic growth pattern remains. As N increases, the amount of inner-loop works still grows proportionally to N^2.

# Analysis and Reflection

A mentioned earlier, big O is not concerned with defining the exact number of operations. Its looking for time complexity as N grows and more specifically, we are looking to define a growth pattern. You can add constants all day without changing the shape of the graph. O(N) describes linear growth. If the array size doubles, the amount of work also doubles. O(N^2) is a quadratic growth pattern. If N doubles, the amount of work grows four times. Sequential loops and nested loops are different because sequential loop costs are added together (as mentioned in par 3). Two loops that each execute N times require 2N operations, which in big O (removing the constants) O(N). With nested loops, one loop runs inside the other so in that case the work is multiplied. If both loops execute about N times we are looking at O(N^2). Understanding time complexity becaomes more important the larger the dataset. Seemingly small differences with say 10 elemnts dont seem nearly as small when you are working with 10,000 elements. 


