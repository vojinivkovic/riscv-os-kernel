# RISC-V OS Kernel

A C++ implementation of a **preemptive, multithreaded operating-system kernel for RISC-V**, featuring a custom memory allocator, thread management, scheduling, synchronization, system calls, and interrupt-driven I/O.

The kernel uses a **preemptible kernel architecture**, where user threads have separate user and kernel stacks, allowing kernel execution to be interrupted and another thread to be scheduled.

## Architecture

The kernel consists of several core subsystems:

* Thread management and scheduling
* Trap and interrupt handling
* Context switching
* Synchronization primitives
* Custom dynamic memory allocation
* Kernel object pools
* Buffered console I/O
* System-call interfaces

The system operates in a single address space with the kernel and statically linked user application sharing the available memory.

## Threading and Scheduling

Threads are represented by thread control blocks containing execution context, scheduling state, stack information, and synchronization data.

The scheduler provides:

* Preemptive, time-sliced scheduling
* Thread creation, termination, joining, and dispatching
* Thread sleeping and timer-based wake-up
* Kernel-mode preemption

Context switching is implemented in RISC-V assembly and saves and restores the execution state required to resume a thread.

## Trap and Interrupt Handling

System calls, timer interrupts, and console interrupts are handled through a common RISC-V trap mechanism.

The kernel supports:

* System calls through the RISC-V `ecall` mechanism
* Timer-driven thread preemption
* Timer-based thread wake-up
* Console interrupts

## Synchronization

Synchronization is implemented using **semaphores** with blocked-thread queues. Atomic RISC-V instructions are used to protect semaphore operations in the preemptible kernel.

## Memory Management

The kernel includes a **custom dynamic memory allocator implemented from scratch** for managing kernel heap memory.

The allocator uses a **best-fit strategy** and explicitly manages free memory blocks, supporting:

* Block splitting during allocation
* Coalescing of adjacent free blocks
* Free-space and largest-block queries

Thread and semaphore objects are managed through dedicated **kernel object pools**, which obtain additional memory from the custom allocator when required.

## Console I/O

Console I/O uses **buffered producer/consumer kernel threads**. Separate input and output buffers are combined with wait queues so that threads can block when data is unavailable.

## System Calls

The kernel exposes three interfaces:

* **C++ API** — object-oriented wrappers for threads, semaphores, periodic threads, and console I/O
* **C API** — procedural system-call interface
* **ABI** — low-level RISC-V system-call interface

Implemented functionality includes memory allocation, thread management, synchronization, sleeping, and console I/O.

## Technologies

* C++
* RISC-V assembly
* Operating-system internals
* Preemptive multithreading
* Scheduling and synchronization
* Memory management
* Trap and interrupt handling
* System calls and ABI
* Memory-mapped I/O
