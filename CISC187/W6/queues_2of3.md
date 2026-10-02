NOTE: For all of the past assignments I have been including my code every step. I now realize that isn't necessary and that it is essentially a guided build until later on - in this case part 10 is where we demonstrate everything. Apologies for that. For this reason I will only be including the analysis for many parts as I build within C++ shell and only include my code and output in part 10. 

# Implementing Circular Queues in C++

## Part 1 — Trace Queue Operations

enqueue(10): Value Returned: NA | Queue: [10] | Front: 10 | Size: 1  
enqueue(20): Value Returned: NA | Queue: [10, 20] | Front: 10 | Size: 2  
enqueue(30): Value Returned: NA | Queue: [10, 20, 30] | Front: 10 | Size: 3  
dequeue(): Value Returned: 10 | Queue: [20, 30] | Front: 20 | Size: 2  
enqueue(40): Value Returned: NA | Queue: [20, 30, 40] | Front: 20 | Size: 3  
enqueue(50): Value Returned: NA | Queue: [20, 30, 40, 50] | Front: 20 | Size: 4  
dequeue(): Value Returned: 20 | Queue: [30, 40, 50] | Front: 30 | Size: 3  
enqueue(60): Value Returned: NA | Queue: [30, 40, 50, 60] | Front: 30 | Size: 4  

### Analysis  
The final front element is 30. The final queue size is 4. The remaining elements would be removed in the order 30, 40, 50, 60. That demonstrates FIFO because the first value added is always the first one removed.

---

## Part 2 — Why Not Shift the Array?

### Analysis  
1: About N - 1 elements may need to move during one dequeue().
2: The shifting approach is O(N).
3: Removing all N elements requires O(N²) total work because each dequeue may shift many remaining elements.
4: Advancing frontIndex is better because it removes an element in O(1) without moving the others.

---

## Part 3 — Implement a Circular Queue

Implemented

---

## Part 4 — Queue State and Invariants

1: count == 0 means the queue is empty because there are no elements currently stored. Since count tracks the number of elements directly, zero means there is nothing to remove.  
2: count == CAPACITY means the queue is full because every available array position is being used. With a capacity of 10, a count of 10 means no more elements can be added.  
3: frontIndex == rearIndex can represent either an empty or full queue in a circular implementation. This happens because both indexes can wrap around and eventually point to the same position.  
4: count removes this ambiguity because it tells us exactly how many elements are stored. If it is 0 the queue is empty, and if it equals CAPACITY the queue is full.  

---

## Part 5 — Implement enqueue()  

1: ++rearIndex by itself would eventually move rearIndex past the end of the array. Using (rearIndex + 1) % CAPACITY makes the index wrap back to 0. This gives it the circular functionality. As long as capacity is a positive integer, we are good to go. 

---

## Part 6 — Implement dequeue()

1: We move frontIndex instead of shifting every element so we do not have to perform a bunch of unnecessary operations. The next value simply becomes the new front.

---

## Part 7 — Implement front()

1: front() just looks at the first value in the queue. dequeue() removes that value and moves the front forward.

---

## Part 8 — Demonstrate Circular Wraparound

### Queue State Trace  
Initial:      frontIndex: 0 | rearIndex: 0 | count: 0 | Queue: []  
enqueue(10):  frontIndex: 0 | rearIndex: 1 | count: 1 | Queue: [10]  
enqueue(20):  frontIndex: 0 | rearIndex: 2 | count: 2 | Queue: [10, 20]  
enqueue(30):  frontIndex: 0 | rearIndex: 3 | count: 3 | Queue: [10, 20, 30]  
enqueue(40):  frontIndex: 0 | rearIndex: 4 | count: 4 | Queue: [10, 20, 30, 40]  
enqueue(50):  frontIndex: 0 | rearIndex: 0 | count: 5 | Queue: [10, 20, 30, 40, 50]  
dequeue():    frontIndex: 1 | rearIndex: 0 | count: 4 | Queue: [20, 30, 40, 50]  
dequeue():    frontIndex: 2 | rearIndex: 0 | count: 3 | Queue: [30, 40, 50]  
enqueue(60):  frontIndex: 2 | rearIndex: 1 | count: 4 | Queue: [30, 40, 50, 60]  

1: Wraparound occurs when rearIndex reaches the end of the array and moves from index 4 back to index 0.
2: The next position after the last array index is 0 because modulo wraps the index back to the beginning.
3: Physical array order can differ from logical queue order because the queue can wrap around and reuse earlier array positions.
4: Modulo makes this possible by wrapping the index back to 0 when it reaches the capacity. Example: with capacity of 5, (4 + 1) % 5 = 0, so the index moves from 4 back to 0.

---

## Part 9 — Logical Position vs. Physical Position

### Position Calculations

### Analysis

---

## Part 10 — Test the Complete Circular Queue

### Complete Program

### Test Results

---

## Part 11 — Complexity Analysis

### Operation Complexity

### Circular Queue vs. Shifting Queue

---

## Part 12 — FIFO Correctness

### Operation Results

### Analysis

---

## Part 13 — Queue Applications

### Scenario A — Print Server

### Scenario B — Server Requests

### Scenario C — Undo

### Scenario D — Breadth-First Search

### Scenario E — Function Calls

---

## Part 14 — Queue and Breadth-First Search

### BFS Visit Order

### Queue Trace

### Complexity

---

# Analysis and Reflection
