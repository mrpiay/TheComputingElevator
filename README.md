The **Computing Elevator** is a unique and engaging web-based educational tool designed to introduce the basics of computer architecture and low-level programming through a visual and interactive experience.

The simulator represents a minimal computer architecture illustrated as **twin towers** (representing the memory) connected by an **elevator** (representing the CPU).

- A **program** consists of instructions and data written on the corresponding floors.
- The **CPU** is visualized as an elevator moving between floors, following the **fetch-decode-execute** cycle.
- The elevator displays the value of the **accumulator** register (ACC) as each instruction is processed.
- Programs always start at instruction floor **0** and proceed **sequentially** unless a jump instruction is found.
- The internal **program counter** (PC) is updated automatically and indicates the next instruction floor where the elevator will stop.
- In addition to **direct addressing** mode, where instructions directly reference a specific data floor, **indexed addressing** is also supported. Instructions in the format **7X** perform the same operation as instructions in the format **0X**, but the data floor is calculated as **X + Data[0]**. Data floor 0 acts as the **index register** (see Example 11).

The Computing Elevator supports instructions encoded with **two denary digits**, as defined in its **Instruction Set**, emulating a machine code programming language. With only 10 instruction and data floors available by default, it encourages users to get creative, designing their own problems and solving them efficiently, like generating a full Fibonacci sequence in just 9 instructions (see Example 10).

Users can design their own programs by entering instructions and data on the respective floors. These programs can be **saved** to a JSON file and **loaded** later, facilitating sharing and experimentation.

## New Feature in v2.0

The Computing Elevator **v2.0** introduces a new feature called **Paging**, which allows users to extend the addressable memory space beyond the initial 10 floors, enabling more complex and creative programming challenges.

Paging divides the memory into multiple blocks or pages, **each containing 10 floors**. This feature is implemented through the **9X** instruction, which allows selecting the current page:

- **90**: Page 0 (floors 0-9)
- **91**: Page 1 (floors 10-19)
- ...
- **99**: Page 9 (floors 90-99)

With paging, the second digit of an instruction (**X**) refers to a floor within the current page (block of 10 floors). Additionally, the index register has been moved to floor 0 (it was floor 9 in v1.0).

To set the number of pages used in the simulator, add **?blocks=n** to the URL, where **n** should be a number between 1 and 10. If this parameter is not added, the simulator will default to 1 page (10 floors).

## Instruction Set

All instructions are **two digits** long for consistency.

### Memory & Arithmetic

| Code | Instruction             | Description                                                                              |
|------|-------------------------|------------------------------------------------------------------------------------------|
| 0X   | ACC ← DATA[X]           | Copy the value stored on data floor X into the elevator                                  |
| 1X   | ACC ← ACC + DATA[X]     | Add the value stored on data floor X to the elevator                                     |
| 2X   | ACC ← ACC - DATA[X]     | Subtract the value stored on data floor X from the elevator                              |
| 3X   | DATA[X] ← ACC           | Copy the value from the elevator into data floor X                                       |
| 7X   | ACC ← DATA[X + DATA[0]] | Copy the value stored on data floor (X + DATA[0]) into the elevator (indexed addressing) |

### Program Flow

| Code | Instruction              | Description                                                      |
|------|--------------------------|------------------------------------------------------------------|
| 4X   | PC ← X                   | Go to instruction floor X                                        |
| 5X   | IF ACC == 0 THEN PC ← X  | If the elevator value is 0, go to instruction floor X            |
| 6X   | IF ACC < 0 THEN PC ← X   | If the elevator value is less than 0, go to instruction floor X  |

### Input/Output

| Code | Instruction  | Description                                              |
|------|--------------|----------------------------------------------------------|
| 80   | ACC ← INPUT  | Copy the value entered by the user into the elevator     |
| 81   | OUTPUT ← ACC | Copy the value from the elevator into the output         |
| 82   | STOP         | Stop the program                                         |

### Other

| Code | Instruction   | Description      |
|------|---------------|------------------|
| 9X   | SET PAGE TO X | Switch to page X |
