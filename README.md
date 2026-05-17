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

### Synchronization
- Output must **clearly demonstrate synchronization** between producers and consumer.
- Correct handling of mutual exclusion and buffer state is required.

---

## Program Structure

```
.
├── main.c        # Process creation, synchronization, and cleanup
├── buff.h        # Shared buffer definition
├── buff.c        # Buffer operations
└── README.md     # Assignment documentation
```

---

## Compilation & Execution

Compile using GCC:

```bash
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
