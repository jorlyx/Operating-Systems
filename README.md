Operating Systems Coursework — Task A & Task B  
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
This is a university coursework project i did in my First Year therefore i cannot upload the code as it might be used in future coursework

Task A: Command-line file operations in Java (re-creating UNIX utilities).  

Overview
Task A re-implements a subset of UNIX-style utilities (cat, wc, sort, uniq) and the pipe operator (|) using pure Java and its standard library.  
It behaves like a simple shell where commands can be chained with pipes, processing text files line-by-line without using external processes.

How It Works

1. The program displays a prompt (>>) and waits for user commands.  
2. Input lines are split on the pipe (`|`) symbol; each segment is processed in sequence.  
3. Each command works with an in-memory List<String> of lines:
   - cat <file> — reads a file into memory.  
   - sort — sorts lines in ascending (case-sensitive) order.  
   - uniq — removes only **consecutive** duplicate lines.  
   - wc — counts lines, words, and bytes.  
     - Option -l returns only line count.  
     - Counts are UTF-8 based for byte accuracy.  
4. Piped commands pass their output to the next command in the chain.  
5. Errors (e.g. invalid files, directories, unsupported commands) throw IllegalArgumentException with messages formatted exactly as required by the specification.  
6. Output from the final command is printed and also stored in bufferOutput for testing.  

Example:
>> cat taskA.csv | sort | uniq | wc -l
22
Edge Cases Handled
Blank input lines are ignored.
Empty files return 0 0 0 in wc.
Directories or missing files trigger descriptive error messages.
Commands with missing arguments (e.g., cat with no filename) are caught.

Features
Fully supports command chaining with |.
Implements cat, sort, uniq, wc, and wc -l.
Strict file validation (exists, isFile, directory check).
UTF-8 encoding for accurate byte counting.
Graceful handling of empty files and blank input.
Internal output buffer for unit testing.

Project Structure 
src/
 └─ task1/
     └─ TaskOne.java
taskA.csv        

Skills Demonstrated
Java I/O (API: BufferedReader, FileReader).
String and stream manipulation.
Command-line parsing and pipeline chaining.
Exception handling and validation logic.
Design for testability (buffered outputs).

Possible Improvements
Support for more UNIX flags (sort -r, uniq -c, wc -w, wc -c).
True stdin stream reading to emulate interactive shell pipelines.
Replace System.out.println with a structured logging system.
Enhanced error messages (permissions, encoding mismatches).
Unit test coverage for malformed commands and mixed pipes.

Task B — Multithreaded Scheduling Simulator
Overview
Task B implements a simplified Operating System process scheduler in Java.
It models how an OS handles process creation, CPU execution, and scheduling under multiple algorithms — FCFS, Non-Preemptive Priority, and Round Robin.
Each simulated process runs a small Python script, with Java threads representing system components such as the CPU, Dispatcher, and Process Creator.

How It Works
Main class: ProcessScheduler.Main
Core Execution Flow
Input File:
The input file (e.g. InputScripts1.txt) defines all processes:
PID,Priority,PythonScript.py
P01,1,process1.py
P02,7,process2.py
P03,6,process3.py
P04,3,process0.py

JobQueue
Reads and parses the file.
Creates Process Control Blocks (PCBs) for each process.
Calculates CPU burst time based on script size and measured execution speed.

ProcessCreator (Thread)
Moves PCBs from the JobQueue to the ReadyQueue.
Sets their state to "ready" and records arrival time.

Dispatcher (Thread)
Retrieves ready processes one at a time and hands them to the Scheduler.

Scheduler
Implements three scheduling algorithms:
FCFS (First-Come, First-Served): Executes in order of arrival.
Priority (Non-Preemptive): Executes highest priority process to completion.
Round Robin: Time-sliced execution using a configurable quantum (ms) with context switch tracking.

CPU (Thread)
Sets the PCB’s state to "running".
Executes the assigned Python script using ProcessBuilder("python3", path).
Simulates CPU work by sleeping for the burst duration.
Reads output, sets the PCB’s result, calculates execution time, and marks the process "terminated".

EventLog
Records “Running” and “Complete” messages and tracks all PCBs.
Completion Order
After all threads finish, a formatted summary shows each process’s result, context switches, and completion order.

Features
Fully-threaded design modelling OS behaviour:
JobQueue → ProcessCreator → Dispatcher/Scheduler → CPU
Implements three scheduling algorithms:
FCFS
Non-Preemptive Priority
Round Robin
Dynamic CPU burst estimation from script size.
Real Python script execution with simulated burst delays.
Context switch counting in Round Robin.
Synchronised queue access and proper thread joining.
Event logging and formatted completion summaries.

Project Structure (Task B)
src/
 └─ ProcessScheduler/
     ├─ Main.java
     ├─ JobQueue.java
     ├─ ProcessCreator.java
     ├─ Dispatcher.java
     ├─ Scheduler.java
     ├─ CPU.java
     ├─ ProcessControlBlock.java
     └─ EventLog.java

InputScripts1.txt   # Example input list
process0.py ...     # Provided Python scripts

Example Usage
Inside Main.java, parameters are configured as:
String[] parameters = { "InputScripts1.txt", "RR", "50" };
Then run:
javac -d out $(find src -name "*.java")
java -cp out ProcessScheduler.Main

Example output:
>> P01: Running
P01: Complete, Context Switches: 0, Output: Sum is 9, Execution Time: 70ms
P02: Running
P02: Quantum exceeded
P02: Complete, Context Switches: 1, Output: [10, 30, 50, 70], Execution Time: 261ms
...
Completion order:
P01: Complete, ...
P02: Complete, ...

Skills Demonstrated
Thread-based concurrency and synchronisation in Java.
Process scheduling algorithms: FCFS, Non-Preemptive Priority, Round Robin.
Operating System principles: PCB, JobQueue, ReadyQueue, CPU bursts, context switches.
Process control: Running external programs via ProcessBuilder.
Performance simulation: Estimating CPU burst times.
Event logging and lifecycle tracking.

Possible Improvements
Refine Round Robin logic to more accurately enforce time quantum slices.
Add Preemptive Priority and Shortest Job First (SJF) variants.
Introduce configurable settings (e.g., JSON input for algorithms and quantum).
Include turnaround, waiting, and response time metrics.
Add timeout or error handling for failed Python scripts.
Implement visual or textual Gantt chart for scheduling visualization.
Modularise logging for easier analysis or export to file.
