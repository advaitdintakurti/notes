
>All information in a system is represented as a bunch of bits.

```c
#include <stdio.h>

int main() {
    printf("hello, world\n");
    return 0;
}
```

The `hello.c` program is stored in a file and is organized as 8-bit chunks called bytes. Each byte has an integer value that corresponds to some character.

![[hello-world-ascii.png]]

> ASCII (American Standard Code for Information Interchange) is used above.

Files that only contain ASCII characters are called *text files*, others are called *binary files.*

## Compilation
In order to run hello.c on the system, the individual C statements must be translated into a sequence of low-level machine-language instructions.

These instructions are then packaged in a form called an executable object program and stored as a binary disk file. Object programs are also referred to as executable object files.

![[compilation-system.png]]
The `gcc` compiler reads the source file `hello.c` and translates it into an executable object file `hello`.

#### Preprocessor (`cpp`)
Modifies the original C program according to directives that begin with the ‘`#`’ character.

For example, the`#include <stdio.h>` command of `hello.c` tells the preprocessor to read the contents of `stdio.h` and insert it directly into the program text. The resulting C program has a `.i` suffix.

#### Compiler (`cc1`)
The compiler (`cc1`) translates the text file `hello.i` into the text file `hello.s`, which contains an assembly-language program. This program includes the following definition of function main:
![[hello-world-asm.png|300]]

#### Assembler (`as`)
The assembler (`as`) translates `hello.s` into machine-language instructions, packages them in a form known as a *relocatable object program*, and stores the result in the object file `hello.o`, which is a binary file.

#### Linker (`ld`)
Our `hello.c` program calls the `printf` function, which is part of the standard C library. The printf function resides in a separate precompiled object file called `printf.o`, which must be merged with `hello.o`. The linker (`ld`) handles this merging. The result is the hello file, which is an executable object file that is ready to be executed.

#### Understanding compilation systems helps with:

- Writing code that is more optimized.
- Understanding link-time errors which are hard to catch.
- Avoiding security holes.

### Execution:
The C program `hello.c`  has been converted to an executable `hello`  by the compilation process. To run the executable, we type its name on a **shell.**

```
> ./hello
hello, world
```

The shell is a command-line interpreter that prints a prompt, waits for you to type a command, and then performs the command.

In this case, the shell loads and runs the `hello` program and then waits for it to terminate.

## Hardware Organization

![[hw-organization.png]]

#### Buses
Buses carry information back and forth between components. They are designed to transfer fixed-size chunks of bytes known as words. Most machines today have word sizes of either 4 bytes (32 bits) or 8 bytes (64 bits).

#### I/O Devices
Input/Output (I/O) devices connect the system to the external world. Each I/O device is connected to the I/O bus by either a *controller* or an *adapter*. 
- **Controller:**  Chipsets on the device itself or on the motherboard.
- **Adapter**:  Card that plugs into a slot on the motherboard.

#### Memory
The main memory is a temporary storage device that holds both a program and the data it manipulates while the processor is executing the program. Physically, main memory consists of a collection of dynamic random access memory (DRAM) chips. Logically, memory is organized as a linear array of bytes, each with its own unique address (array index) starting at zero.

#### Processor
The **Central Processing Unit (CPU)** is the core component that interprets and executes instructions stored in main memory. At its core lies the **Program Counter (PC)**, a register that holds the address of the next instruction to execute.

From power-on to power-off, the processor continuously executes the instruction at the PC, interprets it, performs the required operation, and updates the PC to the address of the next instruction, which may not always be contiguous.

**Instruction Set Architecture (ISA):** Defines the effects of machine-code instructions, ensuring instructions execute in sequence through a series of steps:
 - **Fetch:** Read the instruction from memory at the PC.
 - **Decode:** Interpret the instruction bits.
 - **Execute:** Perform the specified operation.
 - **Update:** Adjust the PC to the next instruction.
    
**Register File:** A small, fast storage containing multiple word-sized registers with unique names.

**Arithmetic/Logic Unit (ALU):** Performs computations to produce new data or address values.

Here are some examples of the simple operations that the CPU might carry out at the request of an instruction:
- **Load:** Transfer a byte or word from memory to a register.
- **Store:** Transfer a byte or word from a register to memory.
- **Operate:** Use the ALU to compute results from two registers and store them in another.
- **Jump:** Update the PC with a specific address from the instruction.


### Running `hello`

![[running-hello-1.png|600]]
Initially, the shell program is executing its instructions, waiting for us to type a command. 

As we type `./hello` on the keyboard, the shell program reads each one into a register and then stores it in memory

When we hit the enter key on the keyboard, the shell loads the executable `hello` file by executing a sequence of instructions that copies the code and data in the `hello` object file from disk to main memory.

![[running-hello-2.png|500]]

Using a technique called *direct memory access (DMA)*, the data travels directly from disk to main memory.

![[running-hello-3.png|550]]
Once the code and data in the `hello` object file are loaded into memory, the processor starts executing the instructions in the `hello` program’s main routine. These instructions copy the bytes in the `hello, world\n` string from memory to the register file, and from there to the display device, where they are displayed on the screen.

### Cache
Copying information from one place to another is a big overhead that slows down the process of execution, and thus we need to make the copy operations *as fast as possible.*


> Because of physical laws, larger storage devices are slower than smaller ones. A typical register file stores only a few hundred bytes of information, as opposed to billions of bytes in the main memory. However, the processor can read data from the register file almost 100 times faster than from memory. This causes a  processor–memory gap in speed.

To deal with the difference in speed system designers include smaller, faster storage devices called *cache memories* (or simply *caches*) that serve as temporary staging areas for information that the processor is likely to need in the near future.
![[cache.png]]

The cache are divided into tiers, with (inversely) varying sizes and access speeds.
- **L1 Cache:**  Holds tens of thousands of bytes and can be accessed nearly as fast as the register.
- **L2 Cache:**  Holds 10-100 times as much as the L1 cache, but is also around 5 times slower.
- **L3 Cache:**  Newer and more powerful systems also have a third level of cache.

These caches are implemented with a hardware technology known as static random access memory.

### Memory Hierarchy
The storage devices in a computer system are organized in a *memory hierarchy*.

![[memory-hierarchy.png]]

The main idea of a memory hierarchy is that storage at one level serves as a cache for storage at the next lower level.

## Operating System
The operating system is a layer of software that connects application programs to the hardware.

![[os1.png|800]]

![[os2.png|700]]

The operating system has two primary purposes: 
- To protect the hardware from misuse by runaway applications.
- To provide applications with simple and uniform mechanisms to interact with hardware devices.

The operating system achieves both goals via the fundamental abstractions shown in Figure 1.11: processes, virtual memory, and files. As this figure suggests, files are abstractions for I/O devices, virtual memory is an abstraction for both the main memory and disk I/O devices, and processes are abstractions for the processor, main memory, and I/O devices.

### Processes
A process is the operating system’s abstraction for a running program. Multiple processes can run concurrently on the same system, and each process appears to have exclusive use of the hardware.

![[contextswitching.png]]
A single CPU can appear to execute multiple processes concurrently by having the processor switch among them. The operating system performs this interleaving with a mechanism known as *context switching*.

The context of a process comprises the state information the CPU needs to execute it, including the current values of the program counter (PC), register file, and main memory contents. At any moment, only one process actively runs on the CPU. During a context switch, the operating system saves the current process's context in memory, restores the new process's context, and transfers CPU control to the new process.

For the new process, this switch is seamless, despite the CPU running other processes in between. Context switching gives the illusion to each process that it has exclusive use of the CPU.

### Kernel
Process transitions are managed by the *kernel*, which is always resident in the memory. When an application requires an OS action, such as reading or writing a file, it issues a system call, transferring control to the kernel. The kernel performs the requested operation and returns control to the application. The kernel is not a separate process but a collection of code and data structures used to manage all processes.

### Threads
A process can consist of multiple execution units, called *threads*, each running in the context of the process and sharing the same code and global data.

Threads provide a mechanism by which an application can perform multiple tasks concurrently.

### Virtual Memory
**Virtual memory** is an abstraction that provides each process with the illusion that it has exclusive use of the main memory. Each process has the same uniform address space, which starts at address zero and continues to the largest possible address.

The virtual address space seen by each process consists of a number of well-defined areas, each with a specific purpose.

Starting with the lowest addresses and working up, these areas are:

- **Program code and data**: Initialized from the executable object file
- **Heap**: Expands and contracts dynamically at run time
- **Shared libraries**: Hold the code and data for shared libraries
- **Stack**: Used by the process to store function parameters, local variables, and return information
- **Kernel virtual memory**: The top region of the address space is reserved for the kernel

![[virtualaddressspace.png]]

Virtual memory abstracts RAM allocation, enabling processes to use large address spaces regardless of available physical RAM. It simplifies memory management by providing a uniform address format for each process, independent of hardware. Additionally, it ensures memory protection, preventing one process from accessing another's memory.

### Files
A **file** is a sequence of bytes. All input and output in the system is performed in terms of reads and writes to files, using a set of system calls called *Unix I/O*. Files are stored on external devices such as disks. Each I/O device, such as a keyboard, display, or network, is modeled as a file, and the operating system provides a uniform set of system calls that applications use to read and write files.

## Networks
Modern systems are often linked to other systems by networks. 

**Networks** are a type of I/O device that can send and receive data. When a system copies bytes from its main memory to the network adapter, the data travels across the network to another machine.

Similarly, a system can read data that has been sent from other machines. Systems copy information from one machine to another via global networks such as the Internet.

![[network.png]]



We can use the telnet application to run `hello` on a remote machine. We use a telnet *client* running on our local machine to connect to a telnet *server* on a remote server.

> Telnet is a network protocol that allows users to access a remote computer and communicate with it in a text-based manner. It's short for "telecommunications network"

![[telnethello.png]]

### Amdahl's Law
Amdahl's law states that when we speed up one part of a system, the overall speedup depends on how significant this part was and how much it sped up. Consider a system where an application requires time T<sub>old</sub> to execute. If a part of the system that initially consumes a fraction α of this time is sped up by a factor of k, the overall execution time will be:
> T<sub>new</sub> = (1 - α)T<sub>old</sub> + (αT<sub>old</sub>)/ k = T<sub>old</sub>((1 - α) + α/k)

From this, speedup **S = T<sub>old</sub>/T<sub>new</sub>** can be computed as:
> S = 1/((1 - α) + α/k)

For example, if the part of the system that initially consumed 60% of the time (α = 0.6) is sped up by a factor of 3 (k = 3), the speedup will be: `1/(0.4 + 0.6/3) = 1.67x`.

**Even though a major part of the system has been improved substantially, the net speedup is less than the speedup of the improved part.**

To significantly speed up the entire system, the speed of a very large fraction of the overall system must be improved

One special case of Amdahl's law is when k is set to ∞, meaning a part of the system is sped up to the point where it takes a negligible time.

The speedup in this case is **S<sub>∞</sub> = 1/(1 - α)**

For example, if 60% of the system is sped up to the point where it takes a negligible time, the net speedup will be `1/0.4 = 2.5x`

Amdahl's law can be applied to improving any process, including manufacturing and academics.

## Concurrency and Parallelism
**Concurrency** refers to a system with multiple, simultaneous activities, while parallelism uses concurrency to make a system run faster. **Parallelism** can be exploited at multiple levels of abstraction in a computer system.

### Thread-Level Concurrency
**Concurrency** enables multiple programs or control flows to execute simultaneously, improving efficiency and multitasking capabilities. Modern systems achieve concurrency through processes and threads, where:

- **Processes** are independent programs.
- **Threads** are control flows within a single process, sharing the same memory space.

![[multiprocessors.png|600]]

#### Simulated Concurrency (Uniprocessor Systems):
A single processor rapidly switches among tasks, creating the illusion of simultaneous execution. This allows:
- Multiple users to interact with a system (e.g., many users accessing a web server).
- A single user to perform multiple tasks (e.g., browsing, word processing, streaming music).

#### True Concurrency (Multiprocessor Systems):
Modern systems use multiple processors or cores under a single operating system kernel. This configuration includes:
- **Multi-core Processors:** Several CPU cores on a single chip, each with dedicated L1/L2 caches and shared higher-level caches and memory interfaces.
- **Hyperthreading:** Enables a single core to execute multiple threads by duplicating some CPU components (e.g., program counters, registers) and sharing others (e.g., floating-point units). Hyperthreading optimizes CPU utilization by switching threads on a cycle-by-cycle basis.

**Benefits of Multiprocessing and Hyperthreading**
1. Reduces the need to simulate concurrency, enabling efficient multitasking.
2. Enhances performance for multi-threaded applications by executing threads in parallel.

Thread-level parallelism is essential for modern applications to fully utilize multi-core and hyperthreaded systems, motivating developers to design programs that exploit parallelism effectively.

![[multicore.png|600]]

### Instruction-Level Parallelism
Modern processors achieve **instruction-level parallelism (ILP)** by executing multiple instructions simultaneously, significantly improving performance compared to early microprocessors.

> Pipelining allows different stages of hardware to operate in parallel, each handling part of a separate instruction. This efficient design enables sustained execution rates close to one instruction per clock cycle. Superscalar processors, capable of executing more than one instruction per cycle, dominate modern architectures.

### SIMD Parallelism
Modern processors support **single-instruction, multiple-data (SIMD)** parallelism, where a single instruction performs multiple operations simultaneously. For instance, recent Intel and AMD processors can use SIMD instructions to add eight pairs of single-precision floating-point numbers (`float`) in parallel.

SIMD is particularly effective for applications involving image, sound, and video processing. While some compilers can automatically extract SIMD parallelism from C programs, a more reliable approach is to use specialized vector data types supported by compilers like GCC. This programming style offers greater control and optimization opportunities.

## The Importance of Abstractions in Computer Systems
Abstraction is a fundamental concept in computer science, enabling the creation of simplified interfaces that hide complex details. A well-designed **application program interface (API)** allows programmers to use functionality without understanding its internal workings. Programming languages like Java and C provide mechanisms for abstraction, such as class declarations and function prototypes.

#### Processor Abstractions
The **instruction set architecture (ISA)** abstracts the complexity of processor hardware, presenting a model where machine-code programs execute as if one instruction is processed at a time. In reality, modern hardware executes multiple instructions in parallel while maintaining the sequential execution model. This abstraction ensures compatibility across different processor implementations, accommodating various performance and cost levels.

#### Operating System Abstractions
1. **Files:** Abstract I/O devices, simplifying data storage and retrieval.
2. **Virtual Memory:** Abstracts physical memory, allowing programs to use more memory than physically available.
3. **Processes:** Abstract running programs, enabling multitasking.
4. **Virtual Machines:** Abstract entire computers, including hardware, operating systems, and applications. Originally introduced by IBM in the 1960s, virtual machines allow systems to run programs designed for different operating systems or versions, making them vital for modern computing environments.

---
Next: [[Chapter 2]]