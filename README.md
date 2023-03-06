# Readers-writers-solution
Starve free solution to the readers-writers problem using semaphores.

We'll be using three semaphores and one count variable.

**The three semaphores and the counter variable used are described below:**

mutex                 (Semaphore)    -   Mutual exclusion of readers

readwrite_mutex       (Semaphore)    -   Mutual exclusion of readers andwriters

writer                (Semaphore)    -   To implement starve free solution

read_count            (integer)      -   No of readers

## *Reader:*

When a reader tries to read, first, 'down(writer)' is called. If a writer process is in the critical section, the reader is queued with the other reader and writer processes, in first-in first-out order. 

Then the 'down(mutex)' is called and the readers' count is incremented by 1. If 'read_count' is equal to 1, we call 'down(readwrite_mutex)' so that any writer cannot enter the critical section, when there is a reader in the critical section. Then we call 'up(mutex)' and 'up(writer)' so that the all the readers can enter the critical section.

After executing the critical sections' code, we again call 'down(mutex)' and decrement the 'read_count' by 1. Now we check if 'read_count' is equal to 0, i.e., if all the readers have finished reading. If yes, then 'up(readwrite_mutex)' is called so that if there was any writer in the waiting queue, it can enter the critical section.

Lastly, we call 'up(mutex)'.

## *Writer:*

We call 'down(writer)' and 'down(readwrite_mutex)' to ensure that no other reader or writer enters the critical section. After the code in the critical section is executed, we call 'up(writer)' and 'up(readwrite_mutex)'. Hence, all the writers are also scheduled in first-in first-out order.
