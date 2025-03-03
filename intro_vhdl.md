# Introduction to VHDL

## What is VHDL?

**VHDL (Very High-Speed Integrated Circuit (VHSIC) Hardware Description Language)** is a programming language used to model and design digital circuits. It allows engineers to describe the behavior and structure of electronic systems, primarily for FPGA and ASIC designs. VHDL enables the creation of both the simulation models and the actual hardware descriptions, making it a versatile tool for electronic design.

Here is one of the video's linked in the readme to help you better understand VHDL, as it is not as intuitive of a language as typical programming languages:

[Watch Video Here](https://www.youtube.com/watch?v=-4On2uXk83k)

### Key Features of VHDL:

- **Hardware Description** – Describes the behavior and structure of digital circuits and systems.
- **Concurrent Execution** – VHDL is used to describe parallel hardware processes, which is different from traditional programming languages that are typically sequential.
- **Strongly Typed** – Enforces strict data type rules to prevent errors, making it easier to catch mistakes during simulation.
- **Simulation and Synthesis** – VHDL code can be simulated to verify functionality before physical hardware is created, and it can also be synthesized into a netlist for actual hardware (FPGA or ASIC).

---

## VHDL vs. Verilog: A Quick Comparison

Both VHDL and Verilog are popular hardware description languages, each with strengths depending on the application:

| Feature         | VHDL                                        | Verilog                                     |
| --------------- | ------------------------------------------- | ------------------------------------------- |
| **Syntax**      | Similar to Ada/Pascal                       | Similar to C                                |
| **Readability** | More verbose, easier to understand          | Concise but can be cryptic                  |
| **Usage**       | Preferred in Europe, aerospace, and defense | Popular in the U.S. and commercial industry |
| **Data Types**  | Strongly typed (strict)                     | Weakly typed (flexible)                     |

---

## Basic VHDL Syntax & Structure

A simple VHDL program typically includes the following:
1. **Entity** – Defines the input/output ports of the module, essentially specifying the interface to the outside world.
2. **Architecture** – Describes the internal logic and behavior of the system. It defines how the entity's behavior is implemented.
3. **Signals & Data Types** – Used to model the internal logic of the design and store values.

### Understanding the AND Gate (For Absolute Beginners)

A **logic gate** is a basic building block of digital circuits. It takes one or more inputs and produces an output based on a specific rule.

The **AND gate** is one of the simplest logic gates. Think of it like a strict rule in real life:

👉 Imagine you and a friend want to enter a club, but the bouncer says:  
**"Both of you must have a ticket to get in!"**

- If **both** of you have a ticket, you get in ✅ (Output: 1)
- If one or both of you **don’t** have a ticket, you don’t get in ❌ (Output: 0)

This is exactly how an **AND gate** works in electronics! It only gives an output of **1 (ON)** if **ALL** its inputs are **1 (ON)**.

### How It Works in a Circuit

The AND gate checks two (or more) signals:
- If **both signals are ON (1)**, the result is **ON (1)**.
- If **either signal is OFF (0)**, the result is **OFF (0)**.

This follows a simple rule, shown in its **truth table**:

|Input A|Input B|Output (A AND B)|
|---|---|---|
|0|0|0|
|0|1|0|
|1|0|0|
|1|1|1|
### Why is This Useful?

AND gates are used in many everyday electronics, including:  
✔️ Password systems (both conditions must be met to unlock something)  
✔️ Safety circuits (both a key and a button must be pressed to activate)  
✔️ Arithmetic operations inside computers

In short, an AND gate is like a rule checker: "Only give an output if all required conditions are met!"

Here is a simple example of an AND gate in VHDL:
```vhdl
-- Import the IEEE library for standard logic definitions
library IEEE;
-- Use the STD_LOGIC_1164 package for standard logic signals
use IEEE.STD_LOGIC_1164.ALL;

-- Define the entity (module) called AND_Gate
-- This is the definition of the AND gate, which has two input signals A and B and one output Y
entity AND_Gate is
    -- Declare input ports A and B as standard logic signals (single bits, either '0' or '1')
    Port ( A, B : in STD_LOGIC;
           -- Declare the output port Y, also a standard logic signal
           Y    : out STD_LOGIC);
end AND_Gate;

-- The architecture block defines the internal behavior of the AND gate
-- The name 'Behavior' is just an identifier for this specific architecture, which could have multiple alternatives
architecture Behavior of AND_Gate is
begin
    -- In this process, the output Y is assigned the logical AND of inputs A and B
    -- The '<= ' operator is used for signal assignment in VHDL
    Y <= A and B;
end Behavior;

```

### Explanation of the Example:

- **Library Declaration**: The `library IEEE;` and `use IEEE.STD_LOGIC_1164.ALL;` lines import the IEEE standard logic package, which is used for digital logic operations.
- **Entity Declaration**: The `entity` block defines the external inputs and outputs of the circuit (in this case, two inputs `A` and `B`, and one output `Y`).
- **Architecture Block**: The `architecture` block describes the internal logic of the entity. The `Y <= A and B;` statement implements the AND operation.

---

## Data Types and Operators

VHDL provides a range of data types and operators to work with digital logic and numeric computations.

### Data Types:

- **STD_LOGIC & STD_LOGIC_VECTOR**: Used to represent digital signals (0, 1, Z for high impedance, X for unknown).
- **INTEGER & REAL**: Used for numerical values and mathematical operations.
- **BOOLEAN**: Represents logical True or False values.
- **ARRAYS & RECORDS**: Used to create complex data structures, like arrays of bits or records with multiple fields.

### Operators:

- **Logical Operators**: `AND`, `OR`, `XOR`, `NOT`
- **Arithmetic Operators**: `+`, `-`, `*`, `/`
- **Relational Operators**: `=`, `/=`, `<`, `>`, `<=`, `>=`
- **Concatenation**: `&` is used to combine multiple signals or values into one vector.

### Example:

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity Adder is
    Port ( A, B : in STD_LOGIC_VECTOR(3 downto 0);
           Sum   : out STD_LOGIC_VECTOR(3 downto 0);
           Carry : out STD_LOGIC);
end Adder;

architecture Behavior of Adder is
begin
    Sum <= A + B;
    Carry <= '1' when (A(3) and B(3)) = '1' else '0';
end Behavior;
```

In this example:
- **STD_LOGIC_VECTOR**: This represents a 4-bit vector for the inputs `A` and `B`.
- **Arithmetic and Relational Operations**: The addition `A + B` is performed on two 4-bit inputs, and a carry signal is generated based on the most significant bit.

---

## Commenting and Documentation

To improve code readability and maintainability, it's essential to add comments in your VHDL code. Comments are placed using the `--` symbol:
```vhdl
-- This is a single-line comment

/*
This is a 
multi-line comment
*/
```

---

## Process and Sequential Statements

VHDL supports both **combinational logic** (described using concurrent statements) and **sequential logic** (using the `process` block). Here's an example of using a process for sequential logic:
```vhdl
process (clk)
begin
    if rising_edge(clk) then
        Q <= D;
    end if;
end process;
```

In this example the `process` block is triggered on a clock signal `clk` and implements a flip-flop behavior where the output `Q` follows the input `D` on the rising edge of the clock.

---
