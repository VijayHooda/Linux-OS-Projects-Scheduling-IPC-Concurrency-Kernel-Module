# Operating Systems Projects — Linux: Scheduling, IPC, Concurrency & Kernel Module (C)

**TL;DR**  
A compact, hands-on collection of Linux systems programming assignments implemented in C: benchmarking Linux scheduling policies (SCHED_OTHER/RR/FIFO), classic concurrency problems (Dining Philosophers with semaphores), multiple IPC mechanisms (UNIX sockets, FIFOs, shared memory), and a simple Linux kernel module. Ideal for systems / backend roles — demonstrates low-level programming, synchronization, performance measurement, and kernel-space coding.

---

## Highlights / Achievements 
- Implemented a benchmark comparing `SCHED_OTHER`, `SCHED_RR`, and `SCHED_FIFO` using `pthread_setschedparam` and high-resolution timers to measure CPU-bound thread performance.
- Solved **Dining Philosophers** (multiple variants) using POSIX semaphores and an extra resource (sauce bowl) to demonstrate deadlock avoidance and fairness strategies.
- Implemented IPC patterns: **UNIX domain sockets**, **named FIFOs**, and **shared memory** (sender/receiver examples) showing message passing and synchronization.
- Built a simple **Linux kernel module** for process lookup and kernel logging (demonstrates kernel-space programming basics).
- Produced benchmarking figures and a write-up summarizing methodology and results.

---

## Repo structure
```
.
├─ benchmarking-scheduling_policies/
│  ├─ benchmark.c
│  ├─ makefile
│  ├─ Figure_1.png ... Figure_5.png
├─ dining_philospher-ipc-syscall/
│  ├─ dining_philosphers/
│  │  ├─ dp1.c, dp2.c, dp1_strict.c, dp2_strict.c, makefile
│  ├─ ques2/  (IPC examples: sockets, fifo, shared mem)
│  │  ├─ ds_sender.c, ds_receiver.c (UNIX sockets)
│  │  ├─ fifo_sender.c, fifo_receiver.c (FIFO)
│  │  ├─ sm_sender.c, sm_receiver.c (shared memory)
│  ├─ ques3/
│  │  ├─ a3q3.c (kernel module), Makefile
│  ├─ writeup.pdf
├─ README.md
```

---

## Technologies / Keywords (for ATS)
**Languages & APIs:** C, POSIX, pthreads, semaphores, UNIX domain sockets, FIFO, shared memory, Linux kernel module (KMOD)  
**Tools & Files:** `make`, `gcc`, `insmod`/`rmmod`, `dmesg`, `strace` (optional)  
**Concepts:** concurrency, synchronization, deadlock avoidance, scheduling policies (SCHED_OTHER, SCHED_RR, SCHED_FIFO), IPC, kernel-space vs user-space, performance benchmarking.

---

## Build & Run (quickstart)

> **Note:** Some operations (setting realtime scheduling policies or loading kernel modules) require **root** privileges (use `sudo`) or capabilities (`CAP_SYS_NICE`, etc.).

### 1) Benchmarking scheduling policies
```bash
cd benchmarking-scheduling_policies
make                # builds benchmark
# run as root for SCHED_RR / SCHED_FIFO to succeed when setting priorities:
sudo ./benchmark
# sample output shows time for each policy, and Figures/* contain plotted results
```
**What it does:** spawns three threads configured with different scheduling policies/priorities and measures loop runtime using `clock_gettime`.

### 2) Dining Philosophers (semaphores)
```bash
cd dining_philospher-ipc-syscall/dining_philosphers
make                # builds dp1, dp2 variants
./dp1               # runs a semaphore-based solution
# try dp1_strict / dp2 / dp2_strict variants
```
**Variants:** normal / strict ordering and a variant adding a shared sauce bowl to demonstrate resource hierarchy and deadlock avoidance.

### 3) IPC examples (sockets, FIFO, shared memory)
```bash
cd dining_philospher-ipc-syscall/ques2
make                # builds senders/receivers for different IPC mechanisms

# Example (UNIX socket):
./ds_receiver &     # start receiver (background)
./ds_sender         # send test packets

# FIFO:
./fifo_receiver & 
./fifo_sender

# Shared memory:
./sm_receiver &
./sm_sender
```
**What to check:** data sent/received, ordering, and behavior on concurrent access.

### 4) Kernel module (ques3)
> **Caution:** Kernel modules affect system stability. Load only on a test VM or sandbox.
```bash
cd dining_philospher-ipc-syscall/ques3
make                # builds a3q3.ko if Makefile present
sudo insmod a3q3.ko process="target_name"
dmesg | tail -n 50  # view kernel logs printed by the module
sudo rmmod a3q3
```
**What it does:** scans tasks and logs process info matching the `process` parameter — demonstrates `for_each_process`, `printk`, and kernel module lifecycle hooks.

---

## Expected outputs & artifacts
- Console logs showing philosopher actions, IPC send/recv, and benchmark timings.
- PNG figures produced in `benchmarking-scheduling_policies/` visualizing measured runtimes.
- `writeup.pdf` contains methodology, experiment results, and conclusions (useful for interview discussion).

---

## Suggested resume bullets (drop-in)
- Implemented multi-threaded benchmarking tool in C to compare Linux scheduling policies (SCHED_OTHER, SCHED_RR, SCHED_FIFO); measured and visualized runtimes using high-resolution timers.  
- Designed and implemented Dining Philosophers solutions using POSIX semaphores (deadlock avoidance and fairness strategies).  
- Built IPC prototypes (UNIX sockets, named FIFO, shared memory) for robust inter-process communication and synchronization.  
- Developed a Linux kernel module to enumerate and log processes by name — experience in kernel-space programming and `printk` debugging.

---

## Testing & safety notes
- Use a VM or container for kernel module experiments (`ques3`) to avoid risking host stability.  
- Real-time scheduling (`SCHED_RR`/`SCHED_FIFO`) may require `sudo` or elevated capabilities — run benchmark with root if timing or `pthread_setschedparam` fails.


---

## License & Contact
- Add your preferred LICENSE (e.g., MIT) to make reuse straightforward.  
- For questions or interview context, include your email or LinkedIn in the repo description or profile.

---

