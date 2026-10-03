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

0: (6 + 0) % 8 = 6 → Physical Index: 6  
1: (6 + 1) % 8 = 7 → Physical Index: 7  
2: (6 + 2) % 8 = 0 → Physical Index: 0  
3: (6 + 3) % 8 = 1 → Physical Index: 1  

1: The logical queue can cross the end of the physical array because modulo wraps the index back to 0. This lets the queue continue using open positions without moving the existing elements.

---

## Part 10 — Test the Complete Circular Queue

### Complete Program  
```
#include <iostream>
#include <stdexcept>
using namespace std;

class Queue
{
private:
    static const int CAPACITY = 5;

    int data[CAPACITY];
    int frontIndex;
    int rearIndex;
    int count;

public:
    Queue();

    bool empty() const;
    bool full() const;
    int size() const;

    void enqueue(int value);
    int dequeue();
    int front() const;
};

Queue::Queue()
{
    frontIndex = 0;
    rearIndex = 0;
    count = 0;
}

bool Queue::empty() const
{
    return count == 0;
}

bool Queue::full() const
{
    return count == CAPACITY;
}

int Queue::size() const
{
    return count;
}

void Queue::enqueue(int value)
{
    if (full())
    {
        cout << "Queue is full. Throwing overflow error." << endl; //c++ shell not throwing errr - using simple cout statement
        return;
        //throw overflow_error("Queue overflow");
    }

    data[rearIndex] = value;
    rearIndex = (rearIndex + 1) % CAPACITY;
    count++;
}

int Queue::dequeue()
{
    if (empty())
    {
        cout << "Throwing underflow error." << endl; //c++ shell not throwing errr - using simple cout statement
        return -1;
        //throw underflow_error("Queue underflow");
    }

    int value = data[frontIndex];
    frontIndex = (frontIndex + 1) % CAPACITY;
    count--;

    return value;
}

int Queue::front() const
{
    if (empty())
    {
        cout << "Throwing underflow error." << endl; //c++ shell not throwing errr - using simple cout statement
        return -1;
        //throw underflow_error("Queue is empty");
    }

    return data[frontIndex];
}

int main()
{
    Queue queue;

    cout << boolalpha;

    cout << "Initial queue:" << endl;
    cout << "Queue empty: " << queue.empty() << endl;
    cout << "Queue full: " << queue.full() << endl;
    cout << "Queue size: " << queue.size() << endl;

    cout << endl;

    cout << "Adding values:" << endl;

    for (int value = 10; value <= 50; value += 10)
    {
        queue.enqueue(value);
        cout << "Enqueued: " << value << endl;
    }

    cout << endl;

    cout << "Queue full: " << queue.full() << endl;
    cout << "Queue size: " << queue.size() << endl;
    cout << "Front value: " << queue.front() << endl;

    cout << endl;

    cout << "Testing overflow:" << endl;
    queue.enqueue(60);

    cout << endl;

    cout << "Removing two values:" << endl;
    cout << "Dequeued: " << queue.dequeue() << endl;
    cout << "Dequeued: " << queue.dequeue() << endl;

    cout << "Front value: " << queue.front() << endl;
    cout << "Queue size: " << queue.size() << endl;

    cout << endl;

    cout << "Testing circular wraparound:" << endl;

    queue.enqueue(60);
    queue.enqueue(70);

    cout << "Enqueued: 60" << endl;
    cout << "Enqueued: 70" << endl;
    cout << "Queue Full: " << queue.full() << endl;
    cout << "Queue Size: " << queue.size() << endl;
    cout << "Queue Front: " << queue.front() << endl;

    cout << endl;

    cout << "Removing values:" << endl;

    while (!queue.empty())
    {
        cout << "Dequeued: " << queue.dequeue() << endl;
    }

    cout << endl;

    cout << "Queue empty: " << queue.empty() << endl;
    cout << "Queue size: " << queue.size() << endl;

    cout << endl;

    cout << "Testing underflow:" << endl;
    queue.dequeue();

    return 0;
}
```

### Output  
Initial queue:  
Queue empty: true  
Queue full: false  
Queue size: 0  

Adding values:  
Enqueued: 10  
Enqueued: 20  
Enqueued: 30  
Enqueued: 40  
Enqueued: 50  

Queue full: true  
Queue size: 5  
Front value: 10  

Testing overflow:  
Queue is full. Throwing overflow error.  

Removing two values:  
Dequeued: 10  
Dequeued: 20   
Front value: 30   
Queue size: 3  

Testing circular wraparound and reusing positions:  
Enqueued: 60  
Enqueued: 70  
Queue Full: true  
Queue Size: 5  
Queue Front: 30  

Removing values:  
Dequeued: 30  
Dequeued: 40  
Dequeued: 50  
Dequeued: 60  
Dequeued: 70  

Queue empty: true  
Queue size: 0  

Testing underflow:  
Throwing underflow error.  
 
Normal program termination. Exit status: 0  

---

## Part 11 — Complexity Analysis  

enqueue(): O(1) | Adds one value and advances rearIndex.
dequeue(): O(1) | Removes one value and advances frontIndex.
front():   O(1) | Directly accesses the front value.
empty():   O(1) | Checks whether count == 0.
full():    O(1) | Checks whether count == CAPACITY.
size():    O(1) | Returns count.

1: A shifting dequeue() is O(N) because the remaining elements may all need to move. A circular dequeue() is O(1) because it only advances frontIndex. 
 
2: Removing all N elements from a shifting queue is O(N²). Removing all N elements from a circular queue is O(N).  

---

## Part 12 — FIFO Correctness

1: The dequeue() operations return 5, 10, 15, 20.  
2: If values are enqueued as x1, x2, ... xN, they should be dequeued as x1, x2, ... xN.  
3: This tests correctness because a queue should return values in the same order they were added, following FIFO.  

---

## Part 13 — Queue Applications

A: Print jobs should be processed in the order they arrive. Evidence of FIFO. Queue is appropriate.
B: Server requests should generally be processed in arrival order. Evidence of FIFO. Queue is appropriate.
C: Most recent action should be undone first. Evidence of LIFO. Queue is not appropriate; Stack is better.
D: Earlier discovered vertices should be processed before later ones. Evidence of FIFO. Queue is appropriate.
E: Most recently called unfinished function should complete first. Evidence of LIFO. Queue is not appropriate. Stack is better.

---

## Part 14 — Queue and Breadth-First Search

Visit Order: A, B, C, D, E, F

Start: Processed: -- | Added: A    | Queue: [A]  
1:     Processed: A  | Added: B, C | Queue: [B, C]  
2:     Processed: B  | Added: D, E | Queue: [C, D, E]  
3:     Processed: C  | Added: F    | Queue: [D, E, F]  
4:     Processed: D  | Added: None | Queue: [E, F]  
5:     Processed: E  | Added: None | Queue: [F]  
6:     Processed: F  | Added: None | Queue: []  

FIFO causes level-by-level traversal because vertices discovered first are processed first.

Complexity: O(V + E) because each vertex is visited once and each edge is examined.

Worst-Case Auxiliary Space: O(V) because the queue may need to hold many vertices at once.

---

# Analysis

1: A queue is an ADT because it is defined by FIFO behavior and operations like enqueue() and dequeue(), not by one specific storage method.  
2: FIFO removes the oldest item first, while LIFO removes the newest item first.  
3: Circular indexing is better because we only move frontIndex instead of shifting every remaining element after a dequeue().  
4: frontIndex tracks the next item to remove. rearIndex tracks where the next item will be added. count tracks how many items are stored.  
5: Modulo allows an index to wrap back to 0 after reaching the end of the array.  
6: frontIndex == rearIndex can mean either empty or full, so the indexes alone aren't sufficient.  
7: Count solves this because count == 0 means empty and count == CAPACITY means full.  
8: enqueue() and dequeue() are O(1) because they only update a few values instead of looping through the queue.  
9: Because queues are structured around FIFO, and FIFO handles the oldest request first.  
10: BFS uses FIFO so vertices discovered earlier are processed before later ones, causing the graph to be explored level by level.  

