# Data Structures & Systems Design Assignment

**Candidate:** Yash Nema

**Role:** Software Engineering Intern  

---

# Repository Structure
├── lru_cache.py

├── event_scheduler.py

└── README.md


- **lru_cache.py** → Implementation of the LRU Cache
- **event_scheduler.py** → Implementation of the event scheduling algorithms
- **README.md** → Explanation of the approach and complexity analysis

---

# Problem 1: LRU Cache

## Overview

A **Least Recently Used (LRU) Cache** stores a limited number of key-value pairs.  
When the cache reaches its capacity, the item that has not been used for the longest time is removed.

The cache must support two operations:

- `get(key)` → Return the value if the key exists, otherwise return `-1`.
- `put(key, value)` → Insert or update the key-value pair. If the cache exceeds its capacity, the least recently used item is evicted.

Both operations must run in **O(1)** time complexity.

---

## Approach

To achieve constant time performance, the implementation uses two data structures together:

### 1. Hash Map (Dictionary)

The hash map stores:
key → node reference

This allows direct access to elements in **O(1)** time.

### 2. Doubly Linked List

The doubly linked list maintains the order of usage.
Most Recently Used ←→ ... ←→ Least Recently Used


- When a key is accessed, its node is moved to the front.
- The least recently used node is located at the tail of the list.

When the cache reaches capacity, the tail node is removed.

Using a **doubly linked list** allows nodes to be inserted or removed in constant time.

---

## Time Complexity

| Operation | Time Complexity |
|--------|--------|
| get | O(1) |
| put | O(1) |

---

## Space Complexity
O(capacity)

The cache stores at most `capacity` elements.

---

# Problem 2: Event Scheduler

Events are represented as:
(start_time, end_time)

Example:
[(9, 10), (10, 11), (11, 12)]


Adjacent events where the end time equals the start time are **not considered overlapping**.

---

# Function: can_attend_all(events)

## Approach

To determine whether a person can attend all events, the events are first sorted by start time.

After sorting:

- Compare each event with the previous event.
- If the start time of the current event is less than the end time of the previous event, the events overlap.

If no overlaps are found, the person can attend all events.

---

## Time Complexity
O(n log n)

The sorting operation dominates the runtime.

---

# Function: min_rooms_required(events)

## Approach

To compute the minimum number of meeting rooms required, we track the number of events happening simultaneously.

Steps:

1. Extract start times and end times.
2. Sort both lists.
3. Use two pointers:
   - One pointer tracks meeting starts.
   - One pointer tracks meeting ends.

If a meeting starts before the earliest meeting ends, a new room is needed.  
If a meeting ends before the next meeting starts, a room becomes available.

The maximum number of simultaneous meetings determines the minimum number of rooms required.

---

## Time Complexity
O(n log n)

Due to sorting.

---

## Space Complexity
O(n)

For storing start and end times.

---

# Trade-offs: Hash Map + Doubly Linked List for LRU Cache

A hash map alone cannot track the order of usage, while a linked list alone cannot provide constant-time lookup.

Combining both structures provides:

- **O(1) key lookup**
- **O(1) insertion and deletion**
- Efficient tracking of the least recently used element

This design is widely used in real-world caching systems.

---

# Future Improvements

To assign specific room numbers to events (for example "Room A", "Room B"), the scheduler could use a **priority queue (min heap)** that stores:
(end_time, room_id)

When a meeting ends, its room becomes available and can be reused for the next meeting.

---

# Concurrency Considerations

If multiple threads access the LRU cache simultaneously, race conditions may occur.

To make the cache thread-safe:

- Introduce locks or mutexes to synchronize cache operations.
- Wrap `get()` and `put()` operations with synchronization mechanisms.

More advanced implementations could use **read-write locks** to allow multiple concurrent reads.

---

# Example Usage

```python
from event_scheduler import can_attend_all, min_rooms_required

events = [(9,10), (9,12), (11,13)]

print(can_attend_all(events))
print(min_rooms_required(events))
```
