# Designing a Simple OS Task Manager

GoMyCode checkpoint: Introduction to Operating Systems.

The full answer, with Gantt charts and tables, is in **[OS_Task_Manager.pptx](OS_Task_Manager.pptx)**.

## Part 1: Process Scheduling

| Process | Arrival | Burst |
|---------|---------|-------|
| P1      | 0       | 5     |
| P2      | 1       | 3     |
| P3      | 2       | 1     |

**FCFS**

```
| P1 | P1 | P1 | P1 | P1 | P2 | P2 | P2 | P3 |
0                        5              8    9
```
Waiting time: P1 = 0, P2 = 4, P3 = 6. **Average = 3.33**

**Round Robin (quantum = 2)**

```
| P1 | P2 | P3 | P1 | P2 | P1 |
0    2    4    5    7    8    9
```
Waiting time: P1 = 4, P2 = 4, P3 = 2. **Average = 3.33**

**Which is more responsive?** Round Robin. Both have the same average waiting time,
but in RR every process gets the CPU quickly. The average response time is 1 in RR and 3.33 in FCFS.
In FCFS, the short P3 has to wait behind the long P1.

## Part 2: Process Synchronization

**Problem:** `counter++` is three steps: read, add 1, write. If P1 and P2 both read the
same value before either one writes, one update is lost. The final value should be 200,
but it can end up lower.

**Solution:** use a mutex so only one process changes the counter at a time.

```
mutex lock
counter = 0

process P(i):
    repeat 100 times:
        lock(lock)
        counter = counter + 1
        unlock(lock)
```

## Part 3: Memory Management (3 frames, string 1 2 3 2 4 1 5)

| Algorithm | Page faults |
|-----------|-------------|
| FIFO      | 6           |
| LRU       | 6           |

It is a tie here because the string is short and only page 2 is reused.
In general LRU performs better, because pages used recently are likely to be used again.

## Part 4: Disk Scheduling (head at 53)

| Algorithm | Order                                     | Total movement |
|-----------|-------------------------------------------|----------------|
| FCFS      | 53, 98, 183, 37, 122, 14, 124, 65, 67     | **640**        |
| SSTF      | 53, 65, 67, 37, 14, 98, 122, 124, 183     | **236**        |

**SSTF is more efficient.** It moves the head about 63% less.
Its downside is that requests far from the head can wait a long time (starvation).
