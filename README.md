# Linux Concurrent Producer-Consumer System Using Semaphores

**Course:** Operating Systems and Systems Programming (25CS2104E)  
**Term:** 2026-27, Term-I | Section 11 | Team 1

---

## Team Members
| Roll Number | Name     | Responsibility |
|-------------|----------|----------------|
| 2520090169  | Bhargava | System architecture, circular buffer, producer logic, integration testing |
| 2520090107  | Hanish   | Semaphore logic, consumer thread, GDB/strace debugging |
| 2520090162  | Teja     | Thread management, boundary testing, test case validation |
| 2520090043  | Pardhu   | Makefile, GitHub, performance analysis, code review |

---

## Description
A fully synchronized concurrent producer-consumer system implemented in C on Ubuntu Linux using POSIX semaphores and pthreads. Demonstrates mutual exclusion, process synchronization, bounded buffer management, file I/O logging, and prevention of race conditions and deadlocks — covering all six course outcomes of 25CS2104E.

---

## Project Structure

    ├── Docs/
    │   └── Abstract.pdf
    ├── producer_consumer/
    │   ├── main.c          — argument parsing, thread creation/joining, summary
    │   ├── buffer.c        — circular buffer implementation
    │   ├── buffer.h        — buffer declarations
    │   ├── producer.c      — producer thread logic with lifecycle comments
    │   ├── producer.h      — producer declarations
    │   ├── consumer.c      — consumer thread logic with lifecycle comments
    │   ├── consumer.h      — consumer declarations
    │   ├── semaphores.c    — semaphore init and destroy
    │   ├── semaphores.h    — semaphore declarations
    │   ├── logger.c        — file I/O logging implementation
    │   ├── logger.h        — logger declarations
    │   └── Makefile        — build automation with object files
    ├── screenshots/
    │   ├── 1_project_structure.png
    │   ├── 2_build_success.png
    │   ├── 3_basic_test.png
    │   ├── 4_boundary_test.png
    │   ├── 5_stress_test.png
    │   ├── 6_varying_buffer.png
    │   ├── 7_strace.png
    │   └── 8_gdb.png
    └── README.md

---

## Course Outcome Mapping

| CO | Description | How This Project Covers It |
|----|-------------|---------------------------|
| CO1 | OS as a layered service abstraction — syscall interface, kernel vs user space | Semaphore operations (`sem_wait`, `sem_post`) and thread calls are kernel-mediated syscalls. `strace` is used to observe user-to-kernel transitions. See `7_strace.png` and CO1 comment in `main.c` |
| CO2 | Process control primitives — lifecycle, state transitions | Thread lifecycle mirrors process lifecycle: Created → Running → Blocked (on `sem_wait`) → Ready → Terminated. `pthread_create` and `pthread_join` parallel `fork` and `wait`. See CO2 comments in `producer.c` and `consumer.c` |
| CO3 | IPC mechanisms — shared memory, synchronization | The shared circular buffer serves as the shared memory IPC medium between producer and consumer threads. Three semaphores coordinate access, directly demonstrating IPC synchronization |
| CO4 | Virtual memory — address space, dynamic allocation | The circular buffer occupies the heap segment of the process address space. Thread stacks are allocated separately in virtual memory, demonstrating per-thread address space layout |
| CO5 | File systems and File I/O | `logger.c` implements file I/O using `fopen`, `fprintf`, and `fclose` to write timestamped events to `pc_log.txt`, demonstrating Linux file abstraction and buffered I/O syscalls |
| CO6 | Concurrency and synchronization — threads, semaphores, deadlocks | Primary CO. Full implementation of POSIX threads, counting semaphores (`empty`, `full`), binary semaphore (`mutex`), critical section protection, and deadlock-free design through correct semaphore ordering |

---

## Semaphores Used
| Semaphore | Initial Value | Purpose |
|-----------|---------------|---------|
| empty     | buffer_size   | Tracks available slots — blocks producer when buffer is full |
| full      | 0             | Tracks filled slots — blocks consumer when buffer is empty |
| mutex     | 1             | Binary semaphore for mutual exclusion during buffer access |

---

## Build
    cd producer_consumer
    make

## Run
    ./pc <buffer_size> <num_producers> <num_consumers> <items_per_producer>

## Test Cases
| Test | Command | Purpose |
|------|---------|---------|
| Basic | `./pc 5 3 2 10` | Standard multi-producer multi-consumer run |
| Boundary | `./pc 3 1 1 5` | Single producer, single consumer |
| Stress | `./pc 10 5 5 20` | High load with many threads |
| Small Buffer | `./pc 2 4 4 8` | Forces producer blocking on full buffer |

## Clean
    make clean

---

## OS Concepts Demonstrated
- Bounded buffer with circular array (CO3, CO4)
- POSIX semaphores — `sem_init`, `sem_wait`, `sem_post`, `sem_destroy` (CO6)
- POSIX threads — `pthread_create`, `pthread_join` (CO2, CO6)
- Mutual exclusion and critical section protection (CO6)
- Deadlock prevention through correct semaphore ordering (CO6)
- User-space to kernel-space transition via syscalls (CO1)
- File I/O logging with timestamps to `pc_log.txt` (CO5)
- System call tracing with `strace` (CO1)
- Multi-thread debugging with `GDB` (CO2, CO6)

---

## GitHub Repository
[https://github.com/HanishDev0017/OS-Project](https://github.com/HanishDev0017/OS-Project)