# Producer–Consumer Using Shared Memory and Semaphores

The code in this repository implements the **Producer–Consumer problem** using **shared memory** and **System V semaphores** with strict atomicity requirements.

---

## Requirements & Constraints

### Shared Memory
- Create a shared memory segment using:
  - `shmget`
  - `shmat`
- The **parent process must remove** the shared memory segment using:
  - `shmctl`

### Semaphores
- Use **exactly three semaphores**:
  1. **Mutex semaphore** – controls exclusive access to the shared buffer
  2. **Empty semaphore** – counts empty slots in the buffer
  3. **Full semaphore** – counts full slots in the buffer
- Semaphore operations must be performed using:
  - `semop`
- **Atomic P() and V() operations** must be used
- Do NOT use separate semaphore calls for individual operations

### Processes
- The program must create **three child processes** using `fork()`:
  - **Two producers**
  - **One consumer**

---

## Repository structure

```
.
├── Assignment 1/                       # queue implementation (Queue.c / Queue.h / main.c) and problem statement
├── Assignment_2/
│   └── Krishnan_Ramanarayanan_CS24BTECH11036/
│       ├── main.c                      # process creation, synchronization, and cleanup
│       ├── buff.h                      # shared buffer definition
│       └── buff.c                      # buffer operations
└── README.md
```

This README describes Assignment 2 (producer-consumer).

---

## Compilation & Execution

Compile using GCC:

```bash
cd Assignment_2/Krishnan_Ramanarayanan_CS24BTECH11036
gcc -Wall -o ass2 main.c buff.c
```

Run the program:

```bash
./ass2
```

---

## Synchronization Logic

- **Producers**:
  - Wait on `empty`
  - Acquire `mutex`
  - Insert item into buffer
  - Release `mutex`
  - Signal `full`

- **Consumer**:
  - Wait on `full`
  - Acquire `mutex`
  - Remove item from buffer
  - Release `mutex`
  - Signal `empty`

All semaphore operations are performed using **atomic `semop()` calls**.

---
