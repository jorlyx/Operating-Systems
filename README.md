Operating Systems Coursework — Task A & Task B  

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
