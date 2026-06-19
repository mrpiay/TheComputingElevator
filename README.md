## Overview

The **Computing Elevator** is a unique and engaging web-based educational tool designed to introduce the basics of computer architecture and low-level programming through a visual and interactive experience.

The simulator represents a minimal computer architecture illustrated as **twin towers** (representing the memory) connected by an **elevator** (representing the CPU).

- A **program** consists of instructions and data written on the corresponding floors.
- The **CPU** is visualized as an elevator moving between floors, following the **fetch-decode-execute** cycle.
- The elevator displays the value of the **accumulator** register (ACC) as each instruction is processed.

### How the Elevator Moves

The elevator carries out one instruction at a time, and where it travels next depends on what that instruction does:

- Programs always start at instruction floor **0**. The **program counter** (PC) keeps track of which instruction floor to visit next, so normally the elevator moves **up to the next instruction floor** in sequence.
- While carrying out an instruction the elevator may need a number from memory, so it **crosses over to that data floor** in the data tower to read or store a value, then returns to continue. (With **indexed addressing** — instructions in the format **7X** — the data floor is worked out as **X + Data[0]**, where data floor 0 acts as the **index register**; see Example 11.)
- A **jump** instruction changes the program counter, so the elevator does **not** go to the next floor — it travels straight to the instruction floor named by the jump. A **conditional jump** only does this if the elevator's value meets the condition (for example, it is 0).
- **INPUT** and **OUTPUT** take place on the special **floor −1** at the very bottom of each tower: the elevator drops down there to take a value from the user, or to send the elevator's value to the output.

### Writing Programs with Mnemonics

Computers don't understand words — every instruction has to be expressed as a **code**. In the Computing Elevator each instruction is a **two denary digit code** (a simple kind of **machine code**): the first digit chooses the **operation** and the second digit chooses the **floor** it works on. For example, **13** means "ADD the number on data floor 3".

Writing whole programs in numbers is hard to read, so instead we write our programs using **mnemonics** — short, memorable words such as **LOAD**, **ADD** and **JUMP** — chosen from the **drop-down boxes** on each instruction floor. Each mnemonic stands for one of the codes listed in the **Instruction Set** below, so as your program runs the elevator is still carrying out the underlying machine code.

With only 10 instruction and data floors available by default, the Computing Elevator encourages users to get creative, designing their own problems and solving them efficiently, like generating a full Fibonacci sequence in just 9 instructions (see Example 10).

### Running, Saving and Sharing

Users can design their own programs by choosing instructions from the drop-down boxes and typing data on the respective floors. There are several ways to run a program:

- **RUN** executes the whole program automatically (use the speed selector for normal or top speed).
- **NEXT ▶** runs a single instruction at a time, so you can follow the fetch-decode-execute cycle step by step.
- **◀ BACK** rewinds one step, restoring the full machine state (program counter, accumulator, memory and outputs) so you can replay and review exactly what the elevator did. Stepping forward again replays the recorded states without re-prompting for input.
- **STOP** halts a run, **RESET** returns to the start, and **CLEAR** empties every floor.

As a program runs, a narration bar describes the current instruction in plain English (for example, "Floor 2: ADD the number on data floor 3 to the elevator").

Programs can be **saved** to a JSON file and **loaded** later, or turned into a **shareable link** with the **SHARE** button: the program is encoded into the page URL and copied to the clipboard, so opening the link reproduces the program ready to run — ideal for sending a puzzle to a classmate.

## New in v2.0: Paging

The Computing Elevator **v2.0** introduces a new feature called **Paging**, which allows users to extend the addressable memory space beyond the initial 10 floors, enabling more complex and creative programming challenges.

Paging divides the memory into multiple blocks or pages, **each containing 10 floors**. This feature is implemented through the **9X** instruction (mnemonic **SET PAGE**), which allows selecting the current page:

- **90**: Page 0 (floors 0-9)
- **91**: Page 1 (floors 10-19)
- ...
- **99**: Page 9 (floors 90-99)

With paging, the second digit of an instruction (**X**) refers to a floor within the current page (block of 10 floors). Additionally, the index register has been moved to floor 0 (it was floor 9 in v1.0).

To set the number of pages used in the simulator, add **?blocks=n** to the URL, where **n** should be a number between 1 and 10. If this parameter is not added, the simulator will default to 1 page (10 floors).

## Instruction Set

Programs are written using the **mnemonics** below, picked from the dropdown on each instruction floor (the operation, plus a floor number **X** where one is needed). Every mnemonic maps to a **two digit code** — the machine code the elevator actually runs.

### Memory & Arithmetic

| Mnemonic       | Code | Instruction             | Description                                                                              |
|----------------|------|-------------------------|------------------------------------------------------------------------------------------|
| LOAD X         | 0X   | ACC ← DATA[X]           | Copy the value stored on data floor X into the elevator                                  |
| ADD X          | 1X   | ACC ← ACC + DATA[X]     | Add the value stored on data floor X to the elevator                                     |
| SUBTRACT X     | 2X   | ACC ← ACC - DATA[X]     | Subtract the value stored on data floor X from the elevator                              |
| STORE X        | 3X   | DATA[X] ← ACC           | Copy the value from the elevator into data floor X                                       |
| LOAD INDEXED X | 7X   | ACC ← DATA[X + DATA[0]] | Copy the value stored on data floor (X + DATA[0]) into the elevator (indexed addressing) |

### Program Flow

| Mnemonic     | Code | Instruction              | Description                                                      |
|--------------|------|--------------------------|------------------------------------------------------------------|
| JUMP X       | 4X   | PC ← X                   | Go to instruction floor X                                        |
| JUMP IF 0 X  | 5X   | IF ACC == 0 THEN PC ← X  | If the elevator value is 0, go to instruction floor X            |
| JUMP IF <0 X | 6X   | IF ACC < 0 THEN PC ← X   | If the elevator value is less than 0, go to instruction floor X  |

### Input/Output

| Mnemonic | Code | Instruction  | Description                                              |
|----------|------|--------------|----------------------------------------------------------|
| INPUT    | 80   | ACC ← INPUT  | Copy the value entered by the user into the elevator     |
| OUTPUT   | 81   | OUTPUT ← ACC | Copy the value from the elevator into the output         |

### Other

| Mnemonic   | Code | Instruction   | Description       |
|------------|------|---------------|-------------------|
| STOP       | 82   | STOP          | Stop the program  |
| SET PAGE X | 9X   | SET PAGE TO X | Switch to page X  |
