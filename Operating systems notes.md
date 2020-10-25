Operating systems notes

1. There are many programs on Hard disk. Programs have to be copied in RAM for them to be executed. If there are 500 programs on disk, we may be able to copy only 50 programs in RAM. Which program to copy and keep is decided by a sub-routine or function within the operating system code, called as Long term scheduler.

	* Long term scheduler --> selection of programs from Disk to RAM

2. In short OS is doing resource allocation/resource management. If 100 programs are trying to access the RAM, Resource manager is managing the RAM to decide which 50 of those programs gets access to the RAM.

3. Lets say there are 2 processes/programs P1 and P2, both trying to access (a resource) RAM, which amoung those will be selected to execute is decided by *Short term Scheduler*

4. In short,

	Long term scheduler = to decide which programs should be selected to execute from Disk --> RAM
	Short term scheduler = to decide which program within the RAM should be executed first by --> CPU

5. Operating system is a resource manager. Operating system program is always in RAM. Whenever we boot the computer, OS code is loaded into the RAM. Once the OS program is executed, then that memory is freed in RAM.
		
```
		RAM			
		---------					---------
		|  OS	|					|		|
		| Space	|					|		|
		---------		<-----		|		|
		|		|					|		|
		| User	|					|		|
		| Space	|					|		|
		|		|					|  Disk	|
		|		|					|		|
		---------					|		|
									|		|
									|		|
									|		|
									|		|
									---------

		---------			---------
		| buffer |			| buffer |		
		---------			---------
		Keyboard			Printer

```

6. Each IO device has a buffer associated with it. When we press "A" on a keyboard, then the ascii code of "A" is pushed to the device buffer of the Keyboard. It is then moved from the device buffer to the RAM for execution.

	For ex, lets say we are doing 2 + 3.

	- First 2 will be pushed to Keyboard buffer, then it will be pushed to RAM
	- Then Operand will be pushed
	- Then 3 will be pushed.
	- Once all 3 are pushed to the RAM, CPU will load them into it's own registers.
	- CPU will then push it to the RAM
	- From RAM it will be pushed to one more I/O device buffer, called monitor buffer.
	- From monitor's buffer, it will be displayed to the monitor.

7. What is the difference between a program and a process?

	A program is an executable physically present on the Hard disk, for ex, GoogleChrome.exe. When the exe is double clicked :

	- CPU will check of the program is already present in RAM
	- If not, a copy of the executable is created and loaded into the RAM in user's space.
	- From there it is loaded into the CPU for execution

	A program can have one of the below states :

	- New state - When it is loaded into RAM
	- Ready state - When it is ready to be loaded into CPU
	- Running state - When it is executing
	- I/O state - When it is waiting for an I/O operation to complete
	- Terminated state - When it's execution is completed and it is removed from RAM
	- Suspended ready state - <Revisit>
	- Suspended wait state - <Revisit>

8. Degree of multiprogramming - Maximum number of processes that can be placed in the RAM
	
```
	Let's say our RAM size is 4 GB.
	Let's assume each process is of size 4 Kb.

	The number of processes that can be placed in RAM 
	= 4Gb/4Kb
	= 2^32 bytes / 2^12 bytes
	= 2^20 	number of processes

	Trivia - 
	1 Kb = 2^10 bytes
	1 Mb = 2^20 bytes
	1 Gb = 2^30 bytes
	1 Tb = 2^40 bytes
	1 byte = 8 bits

	4 Gb = 2^2 * 2^30 bytes = 2^32 bytes
	4 Kb = 2^2 * 2^10 bytes = 2^12 bytes

```

9. Types of Operating systems

	- Batch operating systems - Degree of multiprogramming = 1
	- Multiprogramming OS -  Degree of multiprogramming > 1 [Concurrent processing]
	- Multiprocessing OS - More than 1 CPU. Till now we have considered only 1 CPU model. [Parallel processing]

	CPU efficiency = Useful time of CPU/Total time of CPU
	Useful time of CPU = time when it is executing processes.

10. A program is also called as Passive entity, a process from that program is also called Active entity. 

	Let's look at the anotomy of a process.

	- Every program has a lot of functions, all functions are loaded in RAM.
	- The order of function execution is maintained on a STACK.
	- All variables initiated at runtime are allocated on a HEAP.
	- The block of RAM allocated to each process is called as "Process control block" (PCB)

```
	Process control block
	-------------
	|			|
	|	Stack	|
	|			|
	-------------
	|			|						Attributes of a process:
	|	Heap	|						1. Process Id - Unique ID for each process
	|			|						2. Program counter - Counter assigned to each process of same program
	|			|						3. Process state
	-------------						4. General purpose registers
	|			|						5. Priority
	|Static 	|						6. List of open files
	|variables	|						7. List of open devices
	|			|						8. Protection
	|Global		|
	|variables	|
	-------------
	|			|
	|			|
	|	Program	|
	|			|
	|			|
	-------------

```

11. Let's say there are 3 processes P1, P2 and P3 in ready state, waiting to be executed by the CPU. The Scheduler code in the OS will decide, which process will be executed first by CPU. There are various algorithms based on which the scheduler takes this decision. We will discuss them below - 
	
	- First come first serve algorithm - It says the process which was created first in the RAM will be selected by the CPU to be executed first.

	```
		---------
		|	P1	|
		|		|						---------
		---------						|		|
		|	P2	|		-----------> 	|	CPU |	
		|		|						|		|
		---------						---------
		|	P3	|
		|		|
		---------
	```

	- Shortest job first algorithm - This algorithm says, the job with the shortest amount of time should be executed first. Let's say, 

		P1 - takes - 5 ns to execute
		P2 - takes - 3 ns to execute
		P3 - takes - 1 ns to execute

	```
		---------
		|	P1	|						CPU
		|		|						---------
		---------						|		|
		|	P2	|		-----------> 	|	P2  |	
		|		|						|		|
		---------						---------
		|	P3	|
		|		|
		---------
	```

	Let's say P2 is currently getting executed in CPU, and in the meantime a higher priority task P3 comes, here priority is decided by the process which takes least time to execute. In this case, P2 should be PRE-EMPTIED from CPU and P3 should be run. Once P3 finishes, P2 can be executed, if it is eligible to be executed. But how will be remember how to run P2 from the point at which it was stopped.

	For this purpose, we use the following attributes of the process - 

		- Process Id - Unique ID given to each process.
		- Program counter - This will store the location of the last instruction that was executed by the CPU for this process. This is the value from program counter register present in CPU. Next time P2 runs, it will start the execution from this instruction.
		- Process state - When a process is PRE-EMPTIED, OS stores all the values of variables/state from CPU's register. This data from CPU registers is stored in the General purpose registers in the process. Next time then P2 runs, the CPU registers are again filled with these general purpose registers.
		- General purpose registers - Stated above.
		- Priority
		- List of open files
		- List of open devices - Suppose P3 is printing a page, and it is pre-emptied, next time it runs it again starts printing from where it left before being pre-emptied. 
		- Protection - The stack and heap space in the process, should not be accessible by other processes.

12. Context of a Process - The Program control block along with the process attributes are collectively called as the "Context of the process" or execution context.

13. Types of scheduler
	
	- Long term scheduler - Decides which processes should be moved from disk to RAM for execution
	- Short term scheduler - Decides which process should be moved from RAM to CPU for execution.
	- Medium term scheduler - Let's say that there are 4 processes in the CPU. Their priorities are as below. Note that the capacity of RAM is only 4 processes.

	```
		RAM													DISK
		-----------------									---------
		P1 - Priority - 1									|	P5	|
		P2 - Priority - 2		----SWAP OUT P1----->		---------
		P3 - Priority - 3		<----SWAP IN P5------
		P4 - Priority - 4
		-----------------
	```

	Let's say a new process P5 with higher priority 5 is created in the disk. In this case, we have to "SWAP OUT" a process from RAM to disk and "SWAP IN" P5 into the RAM. This process is called SWAPPING. This process of SWAPPING is carried out by Medium term scheduler.

14. The process of PRE-EMPTYING and saving the context of the currently executing process, and loading a higher priority process in the CPU for execution is called as "Context switching".

15. Times associated with a process

	There are 2 concepts related to time. 5 PM is a "Point in time". But time between 5 PM and 7 PM is "Duration in time". Times associated with a process fall in these 2 categories.

		```
		Trivia - A process can 
			a. Either get executed (RUNNING)
			b. Wait for execution (READY)
			c. Waiting for I/O (I/O WAIT)
		```

	Point in time
		- Arrival time - Point in time at which the process has arrived from Hard disk to the RAM
		- Completion time - Point in time when the process has completed the execution.

	Duration in time
		- Turn around time = Completion time - Arrival time, i.e total time spent by the process in the RAM and CPU combined.

```
			      P1	    P2				P1					P3				P1
			|----------|-----------|----------------------|-------------|--------------|
		Arrival																	Completion
		time (t1)	   t2		   t3					  t4			t5			time (t6) 



		For process P1,
		Arrival time = t1
		Completion time = t6
		Turn around time = t6 - t1
		
		d. Waiting time = (t3 - t2) + (t5 - t4) i.e. when the process was not being executed.
		e. Burst time or Execution time - is the time when the process was getting executed, in this case, it is = (t2 - t1) + (t4 - t3) + (t6 - t5)
		f. I/O time - is the time when process is waiting for I/O (not shown above)

		Also, 
		Completion time = Waiting time + Burst time + I/O time 

		g. Response time - ****** Revisit later *******
```

16. Scheduling algorithms
	
	2 types : 
		- Pre-emptive scheduling algorithm
		- Non pre-emptive scheduling algorithm

```
			---------
			|	P1	|						CPU
			|		|						---------
			---------						|		|
			|	P2	|		-----------> 	|	P2  |	
			|		|						|		|
			---------						---------
			|	P3	|
			|		|
			---------

			Let's say there are 2 processes P1 and P2 in the RAM, and P2 is currently executing. Now a new process P3 arrives in the RAM and P3 has a higher priority than P1, P2.

			So in non-pre-emptive type of algorithms, P2 will NOT be pre-empted/interrupted. P2 will finish execution and then P3 will be picked.

			But in pre-emptive type of algoithms, P2 will be pre-empted/interrupted, and P3 will get execution.

		** Scheduling algorithms are only applicable to processes in READY state**

		Processes which are in I/O state are blocked for I/O, and therefore they will not be picked by CPU for processing.


		Consider below scenario,

		P1 - is in - RUNNING state
		P2, P3 are in READY state,
		P4 is in I/O state
		P4 > P2, P3.	

			---------
			|	P1	|						CPU
			|		|						---------
			---------						|		|
			|	P2	|		-----------> 	|	P1  |	
			|		|						|		|
			---------						---------
			|	P3	|
			|		|
			---------
			|	P4	|
			|		|
			---------

		In non pre-emptive algorithms, when P1 finishes execution, P4 will not be picked... 

```

17. Shortest job first scheduling algorithm (SJF)
	
	- It is a non-pre-emptive algorithm
	- The process with the shortest burst time will be given higher priority.
	- This is a priority based algorithm.

```
	Example, 

	|-----------|-------------|-----------|
	|Process ID |Arrival time |Burst time |
	|-----------|-------------|-----------|
	|	1		|	2		  | 3		  |
	|	2		|	3		  |	2		  |
	|	3		|	4		  |	3		  |
	|	4		|	6		  |	1		  |
	|	5		|	8		  |	2		  |
	|-----------|-------------|-----------|

	Calculate the average Turn around time, Waiting time, Throughput and Schedule length. (TAT, WT, Throughput, Schedule length)


	0 <--CPU IDLE--> 2 <-----P1------->	5 <-----P2---->	7 <-P4-> 8 <--P5--> 10 <----P3-----> 13				
	|----------------|------------------|---------------|--------|-----------|---------------|


	At t = 0, no process has arrived in RAM so, CPU is idle.
	At t = 2, P1 arrives and gets executed till t = 5.
	At t = 5, P2 and P3 have already arrived. P2 has lower burst time, so it gets executed first.
	At t = 7, P2 finishes. At this point, processes waiting to be executed are P3 and P4. P4 has lower burst 		   time and gets picked.
	At t = 8, P4 finishes. At this P5 has arrived. P3 and P5 are waiting to be executed. P5 has lower burst 		  time, so it gets picked.  
	At t = 10, P5 finishes, only process remaining is P3, it gets executed.
	At t = 13, P3 finishes.


	* TAT = Completion time - Arriaval time
	* TAT = WT + BT + IO time. For this problem IO time is 0, therefore, WT = TAT - BT

	|-----------|-------------|-----------|---------------|---------------------|-----------------|
	|Process ID |Arrival time |Burst time |Completion time|Turn around time(TAT)|Waiting time (WT)|
	|-----------|-------------|-----------|---------------|---------------------|-----------------|
	|	1		|	2		  | 3		  |	5			  |	 5 - 2 = 3			| 	3 - 3 = 0	  |	
	|	2		|	3		  |	2		  |	7 			  |	 7 - 3 = 4			|   4 - 2 = 2	  |
	|	3		|	4		  |	3		  |	13			  |	 13 - 4 = 9			|	9 - 3 = 6	  |
	|	4		|	6		  |	1		  |	8			  |	 8 - 6 = 2			|	2 - 1 = 1	  |
	|	5		|	8		  |	2		  |	10			  |	 10 - 8 = 2			|	2 - 2 = 0	  |
	|-----------|-------------|-----------|---------------|---------------------|-----------------|

	* Average TAT = (3 + 4 + 9 + 2 + 2)/5
	* Average WT = (0 + 2 + 6 + 1 + 0)/5
	* Schedule length = total time spent by the algorithm in scheduling processes.
					  = Time when last process finished - time when first process started
					  = 13 - 2
					  = 11

	* Throughput = number of processes executed per unit time
				 = number of process/schedule length
				 = 5/11

```

18. Shortest remaining time first (SRTF) scheduling algorithm

	- SRTF is a pre-emptive type of scheduling algorithm. This is the only difference between SJF and SRTF.
		
	In SRTF the process which has shortest burst time is give highest priority. If the current executing process has higher remaining burst time, it will be pre-emptied and the one with highest priority will be executed.

```
		Let's take an example -


		|-----------|-------------|-----------|----------------------|
		|Process ID |Arrival time |Burst time |Remaining burst time  |
		|-----------|-------------|-----------|----------------------|
		|	P1		|	0		  | 7		  |   6                  |
		|	P2		|	1		  |	5		  |   5                  |
		|	P3		|	2		  |	3		  |   2                  |
		|	P4		|	3		  |	1		  |   0                  |
		|	P5		|	4		  |	2		  |   2                  |
		|   P6      |   5         | 1         |   0                  |
		|-----------|-------------|-----------|----------------------|


	0 <-P1-> 1 <-P2-> 2 <-P3-> 3 <-P4-> 4 <-P3-> 5 <-P3-> 6 <-P6-> 7 <--P5--> 9 <--P2--> 13 <--P1--> 19
	|--------|--------|--------|--------|--------|--------|--------|----------|-----------|-----------|

	At,

		t = 0, P1 arrives and starts executing
		
		t = 1, P2 arrives and it till this point the remaining burst time of P1 is 6, which is higher than P2's burst time. P1 is therefore pre-emptied and P2 starts execution.
		
		t = 2, P3 arrives, and it has lower burst time than P2, which has 4 units remaining to be executed. P2 is therefore pre-emptied and P3 starts execution.

		t = 3, P4 arrives and it has low burst time then P3, which has 2 units remaining. P4 therefore executes and P3 is pre-emptied.

		t = 4, P4 completes execution and P5 arrives. At this point both P5 and P2 have equal priority with burst time of 2 units. Anyone can be choose, we'll choose P3 since it came first.

		t = 5, P6 arrives with burst time of 1, and P3 also at this point has remaining burst time of 1, since P3 came first it continues execution.

		t = 6, P3 completes it's execution, now we have P6 as least burst time of 1, so it excutes.

		t = 7, P6 completes it's execution, now we have P5, P2 and P1 left to execute. P5 has least burst time, so it executes.

		t = 9, P5 finishes, next is P2 is left with 4 units of remaining burst time, it executes.

		t = 13, P2 finishes and P1 wit 5 units of remaining burst time, executes.

		t = 19, P1 finishes.


		Now let's compute the Completion time, Turn around time and waiting time for each process.

		|-----------|-------------|-----------|--------|----------|------|
		|Process ID |Arrival time |Burst time | CT     | TAT      | WT   |
		|-----------|-------------|-----------|--------|----------|------|
		|	P1		|	0		  | 7		  |   19   |  19      | 12   |
		|	P2		|	1		  |	5		  |   13   |  12      | 7    |
		|	P3		|	2		  |	3		  |   6    |  4       | 1    |
		|	P4		|	3		  |	1		  |   4    |  1       | 0    |
		|	P5		|	4		  |	2		  |   5    |  5       | 3    |
		|   P6      |   5         | 1         |   7    |  2       | 1    |
		|-----------|-------------|-----------|--------|----------|------|

		## Assuming I/O time of all processes is 0.

		*** Completion time (CT) of a pre-emptive algorithm should always be calculated backwards.
		*** TAT = CT - AT
		*** WT = TAT - BT
		*** Schedule length = 19 - 0 = 19
		*** Throughput = total processes / schedule length = 6 / 19 

```

19. Response time of a process is the time that the process has to wait for it to be executed in the CPU for the first time, after it arrives in RAM. SRTF has better response time than SJF. In general, pre-emptive algorithms have a better response time than non-pre-emptive algorithms. 

20. First come first served (FCFS) scheduling algorithm

	- The process which has the least arrival time will be scheduled first.
	- It is a non-pre-emptive scheduling algorithm.
	- It is not a priority based scheduling algorithm.

```
	|-----------|-------------|-----------|
	|Process ID |Arrival time |Burst time |
	|-----------|-------------|-----------|
	|	P1		|	0		  | 4		  |
	|	P2		|	1		  |	3		  |
	|	P3		|	2		  |	1		  |
	|	P4		|	3		  |	2		  |
	|	P5		|	4		  |	5		  |
	|-----------|-------------|-----------|

	The algorithm is simple, there is no priority assigned to any process based on process properties, rather whichever process comes first in the RAM is selected. Which process arrives at what time in the RAM is not controlled by a process and it is therefore not a process attribute.

	At,
	
		t = 0, P1 arrives in the RAM and it is selected to execute.

		t = 4, P1 completes its execution, and by this time P2, P3 and P4 have arrived in the RAM. But P2 was first to arrive amoung them, there P2 executes first.

		t = 7, P2, finishes, P3 is picked.

		t = 8, P3 finishes, P4 is picked.

		t = 10, P4 finishes, P5 is picked.

		t = 15, P5 finishes


	0 <--P1--> 4 <---P2---> 7 <-P2-> 8 <---P4---> 10 <--P5--> 15
	|----------|------------|------ -|-------------|-----------|


	|-----------|----------|-----------|--------|----------|------|------|
	|Process ID |AT        |   BT      | CT     | TAT      | WT   | RT   |
	|-----------|----------|-----------|--------|----------|------|------|
	|	P1		|	0	   |    4	   |   4    |  4       | 0    |  0   |
	|	P2		|	1	   |	3	   |   7    |  6       | 3    |	 3	 |
	|	P3		|	2	   |	1	   |   8    |  6       | 5    |	 5	 |
	|	P4		|	3	   |	2	   |   10   |  7       | 5    |	 5	 |
	|	P5		|	4	   |	5	   |   15   |  11      | 6    |	 6	 |
	|-----------|----------|-----------|--------|----------|------|------|

	*** For pre-emptive algorithms, WT is same as RT (response time)

```

21. FCFS with Context switching

