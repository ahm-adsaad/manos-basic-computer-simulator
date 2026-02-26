# Mano's Basic Computer Simulator

This program contains a Python implementation of a Mano-style “Basic Computer” simulator.
It reads a program and initial data from text files, loads them into a simulated memory, and lets you step through execution at cycle or instruction level while printing the micro-operations and updated components.

Simulator Overview:

REGISTERS:
AR (Address Register, 12 bits)
PC (Program Counter, 12 bits)
DR (Data Register, 16 bits)
AC (Accumulator, 16 bits)
IR (Instruction Register, 16 bits)
TR (Temporary Register, 16 bits)

FLAGS:
E_flag (Carry flag)
I_flag (Indirect addressing flag)
s_flag (Start/Halt)

COUNTERS / PROFILER:
seq_count (Current micro-step T0, T1, …)
inst_count (Number of executed instructions)
sum_count (Total cycles executed)
avg_count (Average cycles per instruction)
RW_count (Total memory reads + writes)

MEMORY:
64K x 16-bit locations, indexed 0–65535

SUPPORTED INSTRUCTIONS:
Memory reference operations (AND, ADD, LDA, STA, BUN, BSA, ISZ)
Register reference operations (CLA, CLE, CMA, CME, CIR, CIL, INC, SPA, SNA, SZA, SZE, HLT)
I/O instructions are decoded but not implemented.

The simulator loads two files:
1. program.txt
2. data.txt

Each file contains lines of the form:
address_hex value_hex

Example program.txt:
0x100 0x3110
0x101 0x7040
0x102 0x2110
0x103 0x7080
0x104 0x7001

Example data.txt:
0x110 0x0000
0x111 0x00A2
0x112 0x00B3
0x113 0x00C0

EXECUTION COMMANDS:
- next_cycle Executes one micro-step (T0, T1, ...)
- fast_cycle N Executes N micro-steps
- next_inst Executes one full instruction
- fast_inst N Executes N full instructions
- run Executes until the HLT instruction

INSPECTION COMMANDS:
- show_reg_name REG (Displays AC, PC, AR, DR, IR, TR)
- show mem ADDRESS (Displays memory[ADDRESS])
- show mem ADDRESS COUNT (Displays a range of memory)
- show all (Displays all registers and flags)
- show profiler (Displays cycle and instruction counts)
- end (Exits program)

TO RUN WITH COLAB:
1. Connect to the cloud.
2. Update the directory leading to the program.txt and data.txt in the Google Drive.
3. Run all.
4. Proceed with the execution and inspection menu.

Below is a sample program.txt:
0x100 0x3110
0x101 0x7040
0x102 0x2110
0x103 0x7080
0x104 0x7001

Below is a sample data.txt:
0x110 0x0000
0x111 0x00A2
0x112 0x00B3
0x113 0x00C0

Below is a sample output:

Execution Phase Menu
=====================================================================
Please enter your command of interest:
1. next_cycle – executes one clock cycle.
2. fast_cycle N – executes N clock cycles in sequence.
3. next_inst – executes one full instruction cycle.
4. fast_inst N – executes N instruction cycles in sequence.
5. run – executes the whole program until it the HLT instruction is encountered 

Inspection Phase Menu
=====================================================================
Please enter your command of interest:
6. show_reg_name REGISTER_NAME
7. show mem address
8. show mem address memory_count
9. show all
10. show profiler
11. end - To end the program

>> run

Starting program execution...
Instruction in hand: N/A
Micro operation in hand T 0 : AR<- PC
Changed: AR
Instruction in hand: 0x3110
Micro operation in hand T 1 : IR <- M[AR], PC <- PC+1
Changed: IR and PC
Instruction in hand: 0x3110
Micro operation in hand T 2 : D0, …, D7 ← Decode IR(12-14), AR ← IR(0-11), I ← IR(15)
Changed: opp_code and AR and I flag
Instruction in hand: 0x3110
Micro operation in hand T 3 : (direct addressing) No operation
Changed: SC
Instruction in hand: 0x3110
Micro operation in hand T 4 : M[AR] <- AC, SC <-0
Changed: Memory and SC
Instruction in hand: N/A
Micro operation in hand T 0 : AR<- PC
Changed: AR
Instruction in hand: 0x7040
Micro operation in hand T 1 : IR <- M[AR], PC <- PC+1
Changed: IR and PC
Instruction in hand: 0x7040
Micro operation in hand T 2 : D0, …, D7 ← Decode IR(12-14), AR ← IR(0-11), I ← IR(15)
Changed: opp_code and AR and I flag
Micro operation in hand T 3 : AC<- shl AC, AC(10)<-E, E<-AC(15)
Instruction in hand: 0x7040
Changed: E flag and AC and SC
Instruction in hand: N/A
Micro operation in hand T 0 : AR<- PC
Changed: AR
Instruction in hand: 0x2110
Micro operation in hand T 1 : IR <- M[AR], PC <- PC+1
Changed: IR and PC
Instruction in hand: 0x2110
Micro operation in hand T 2 : D0, …, D7 ← Decode IR(12-14), AR ← IR(0-11), I ← IR(15)
Changed: opp_code and AR and I flag
Instruction in hand: 0x2110
Micro operation in hand T 3 : (direct addressing) No operation
Changed: SC
Instruction in hand: 0x2110
Micro operation in hand T 4 : DR <- M[AR]
Changed: DR and SC
Instruction in hand: 0x2110
Micro operation in hand T 5 : AC <- DR, SC <- 0
Changed: AC and SC
Instruction in hand: N/A
Micro operation in hand T 0 : AR<- PC
Changed: AR
Instruction in hand: 0x7080
Micro operation in hand T 1 : IR <- M[AR], PC <- PC+1
Changed: IR and PC
Instruction in hand: 0x7080
Micro operation in hand T 2 : D0, …, D7 ← Decode IR(12-14), AR ← IR(0-11), I ← IR(15)
Changed: opp_code and AR and I flag
Micro operation in hand T 3 : AC<- shr AC, AC(15)<-E, E<-AC(0)
Instruction in hand: 0x7080
Changed: E flag and AC and SC
Instruction in hand: N/A
Micro operation in hand T 0 : AR<- PC
Changed: AR
Instruction in hand: 0x7001
Micro operation in hand T 1 : IR <- M[AR], PC <- PC+1
Changed: IR and PC
Instruction in hand: 0x7001
Micro operation in hand T 2 : D0, …, D7 ← Decode IR(12-14), AR ← IR(0-11), I ← IR(15)
Changed: opp_code and AR and I flag
Micro operation in hand T 3 : S<-0
Instruction in hand: 0x7001
Changed: S and SC

Program halted. HLT instruction encountered.
Below is the program profilier!
Below is the profiler of the process: 
The total cycles executed until now is:  23
The total number of instructions executed till now is:  5
The average cycles per instruction (CPI) is:  4.6
The total memory bandwidth until now, which includes number of memory read and writes is:  2

>> show all

DR = 0x0
AR = 0x1
AC = 0x0
IR = 0x7001
PC = 0x105
TR = 0x0
E flag = 0b0
I flag = 0b0
S flag = 0b0

Sequence Counter = 0

