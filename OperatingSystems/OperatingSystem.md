## Operating System
-------------------

**Chapter1** 
- User View and System View.
- Bit,Byte and Word.
- Operating System = Kernel + Systems Programs.
- Interrupts = H/W Interr.(via System Bus) ; S/W Interr.(via System Call) 
- Virtualization.
- Von Neumann
- Clustering(LAN) and Availability introduced due to redundancy.
- MultiProgramming and Time sharing.
- Virtual Memory, a technique that allows the execution of a process that is not completely in memory **(Chapter 9)**.
- Trap - is a S/W generated interrupt caused either by an error(for example division by zero or invalid memory acess).
- User mode and Kernel mode(supervisor mode,system mode and priviledged mode)
- mode bit/priviledged instruction. 
- A multi-threaded process has multiple program counters, each pointing to the next instruction to execute for a given thread. 
- User and OS processes.
- Cache Coherency(Each CPU also has a local cache in a mulitprocessort environment)
**Chapter2**.
**Chapter3**
- Process = Program in execution.
- Features of Process = Creation,Scheduling,Termination and Communicaiton. 
- Process in memory has - stack(temporary memory for function parameters and local variables),heap(for dynamic memory),data(global variables),text section(contains program code). 
- Same program ~ multiple process.
- Process State ~ New,Running,Waiting(waiting for some event to occur such as an I/O completion),Ready and Terminated. 
- Each process in the operating system is represented by PCB.
- Objective of multiprogramming = max CPU utilization. 
- Objective of time sharing = switch the CPU among processes so frequently that users can interact with each program while it is running.
**Chapter4**
- Chapter 3 = Assumed single thread process.
**Chapter5**
**Chapter6**

**Chapter7** 
	- Deadlocks
**Chapter8**
**Chapter9**
**Chapter10**
**Chapter11**
**Chapter12**
**Chapter13**
**Chapter16**
**Chapter21**
**Chapter23**


### Memory Management(NPTEL)
---
- Single Contiguous Model [No sharing, One process occupies RAM at a time, when one process completes another process is allocated to RAM]
- Parition Mode [Multiple Process occupy the RAM] so the operating system can execute the processes in a time sliced manner or parallely. Partition table is used, it has Memory Address(Base address), Size of the address, process identifier, usage flag. Partition table is also present in the RAM. Processes will be deallocated when execution is done this can lead to **Memory Fragmentation** i.e. free memory blocks are present throughout. **Finding the right fit memory for the new process** `First Fit Algorithm(It could make fragmentation worse),Best Fit(better in terms of fragmentation) it may lead to worse performance since now the OS has to scan through all the free memory blocks.` During deallocation the OS should merge contiguous free blocks into one chunk in the partition table. There is performance degradation in this method due to book keeping and managing partitions. 
-**Virtual Memory** `RAM is divided into page frames typically of 4KBs`.`Process is also split into equal block size`. All prcosses have a process page table which maps block to page frame. The lookup table overhead is partially mitigated using something known as a TLB(Translation Lookaside Buffer) cache.  The process blocks are mapped to user space of the memory and the process page table is in the kernel space of the memory. 
-**Demand Paging**`In Hardisk there will be a area called swap space,all process blocks will be present in the swap space`
-**Page fault Interrupt**`CPU will load the particular process block from swap space into the RAM and also updatet the page table.`
-**Replacement policies of used process blocks from pages in RAM so that the new process blocks can be loaded**`First In First Our,Least recently used,Least frequently used.`The replaced block may need to be written back to the swap area**(swap out)**. The D bit or dirty bit in the page table indicates if a page needs to be written back to disk. If dity bit is 1, indicates the page needs to be written back to disk since its contents were modified. `Proctection bits there determine whether the block is executable,read only.`
-`When a program is executed the compiler adds informaiton like which stack area to use and all, the OS will read the information from the executable.`
`Virtual address space of a process, will have (stack,heap,data[global and static],Text instruction)`
-`How virtual address is mapped to main memory. The processor puts out a virtual 0<=address(v)<=MAX_SIZE, The memory management unit or MMU converts the virtual address to the physical address(in the RAM). In MMU there is PTPR register, this has the base address of the process page table. The virtyal address from the pocessor will have a table index and offset.`
- In 32 bit systems virtual address is of 32 bits. The maximum process size is 2^32=4GB(how). Table Index=20 bits and Offset=12bits(Why is it signed or unsigned).  **Dir(10)|Table(10)|Offset(12)** to solve the issue of 4MB contiguous memory space required in the RAM.
![](os1.png)
- Segmentation ` Offers a more logical split of the program. Virtual memory does not split programs into logical modules, instead splits program into fixed size blocks. So that programs stay together.Again fragmentation issue arises.`
- GDT(global descriptor table) stored in memory is pointed by GDTR(register)
![](os10.png)
![](os9.png)

### Processes
---
- Kernel resides in the lower part of RAM.
- From **MAX_SIZE** to **MAX_LIMIT** of the process's virtual address map kernel is present. Its also mapped in the process page table. 
- **User Space** and **Kernel Space**. Kernel can access both the kernel and user space code. Simple subtraction using **MAX_SIZE** can convert virtual to physical address of the kernel in a process. 
- **3 metadatas:- PCB,Kernel Stack for user process,Page Tables**.Unique for the process. 
- **Stack for user process has** - Local variables and Function calls.
- **Stack for kernel process** - used for local variable of the kernel, context of the process. 
![](os2.png)
- **Copy on Write(COW)*
- **Every Process is created using fork and exec system calls, as a result we get a tree like strucutre. There is a root program. In linux `pstree` prints this tree. The first process is created by the kernel during booting. In linux the command is present in /etc/init.d**
- **exit(0) - Process termination(voluntary). kill(pid,signal) - Process termination(involuntary). signal=SIGTERM,SIGQUIT,SIGINT,SIGHUP**
- **wait() - called in parent, parent goes to block state until one of its chidlren exits, if no children executing then -1 is returned.**
- **wait(&status) - os puts the exit status of the child process.**
- **when a process terminates it becomes a zombie, pcb in OS still exists even though program is no longer executing. this happens so that the paren tprocess can red the child's exit status(through wait system call). When paren treads status zombie entries are removed from OS, is the parent doesn't read status zombe will contnue to exist infinitely`RESOURCE LEAK`** these are typically found by a reaper process. 
- **Orphans - when parent processes terminate before child. processes becomes detached from uer session and runs in the background(Intentional Orphans). these are also called daemons.**
![](os4.png)
![](os5.png)
**gcc hello.c -c; readelf -h hello.o // prints elf headers; readelf -S hello.o // prints section headers**
**A relocatable object file holds sections containing code and data. This file is suitable to be linked with other relocatable object files to create dynamic executable files, shared object files, or another relocatable object. A dynamic executable file holds a program that is ready to execute.**

### Interrupts
---
![](os6.png)
1. H/W Interrupts - Asynchronous. {keyboard,timer,usb dirves}.
The processor has only one INT pin. The interrupt controller shares single pin to multiple devices. The processor checks from the interrupt controller which device called it. CPU uses INTA pin for interrupt ack.  (8259 supports cascading to support more than 8 devices and uses some priority encoding if two devices ask for interrupt at the same time)
![](os7.png)
- Each interrupt is associated with a number. 
- When a H/W interrupt occurs the processor obtains the corresponding number from the interrupt controller. 
- The number is often called IRQ Number. 
- IDTR(Interrupt descriptor table register points to Interrupt handler routine stored in memory)
![](os8.png)
2. Trap - S/W Interrupts, Raised by user program.
3. Exceptions[Faults and Aborts] - generated automatically by the processor, Page Faults(revoverable error),Divide by zero(difficult to recover).
