# Fundamental VHDL Concepts

In VHDL, understanding the core concepts is essential to effectively designing and simulating digital circuits. 

This is recommended viewing at this point because this type of language is not as intuitive as other programming languages: 

[Watch Video Here](https://www.youtube.com/watch?v=-4On2uXk83k&t=37s)

Below are some key fundamental concepts that every VHDL designer should be familiar with:

## Entity and Architecture

**Entity**: The *entity* defines the interface of the digital component, including the inputs and outputs. It acts as a black box description for the component, specifying the type and number of signals that enter and exit the component.
  
Example:
  ```vhdl
  entity AND_Gate is
    port (
      A : in bit;
      B : in bit;
      C : out bit
    );
  end entity AND_Gate;
```

**Architecture**: The _architecture_ describes the internal behavior or structure of the component. It contains the logic for how the signals are processed.

Example:
```vhdl
architecture Behavioral of AND_Gate is
begin
  C <= A and B;
end architecture Behavioral;
```

## Signals vs. Variables

- **Signals**: A signal in VHDL is used for communication between different parts of a design. Signals represent wires or connections and reflect changes in values over time. They are evaluated at specific times during simulation.
- **Variables**: Variables are used within processes and functions to hold temporary data. They have immediate effect during the execution of the process or function and do not represent physical connections.

## Processes and Concurrent Statements

**Processes**: A _process_ is a block of sequential statements that are executed in a specific order. Processes are triggered by changes in the signals listed in the sensitivity list and are the foundation of sequential logic in VHDL.

Example:
```vhdl
process (A, B)
begin
  C <= A and B;
end process;
```

## Conditional Statements (IF, CASE)

**IF Statements**: The _if_ statement is used for conditional execution based on the values of expressions. It is often used in processes to control the behavior of the design.

Example:
```vhdl
if (A = '1') then
  C <= '1';
else
  C <= '0';
end if;
```

**CASE Statements**: The _case_ statement provides a way to select one of several possible conditions based on the value of a signal or variable. It’s more efficient for multiple conditions than a series of _if-else_ statements.

Example:
```vhdl
case (A) is
  when '0' => C <= '1';
  when '1' => C <= '0';
  when others => C <= 'Z';
end case;
```

## Loops and Sequential Execution

**Loops**: VHDL supports various types of loops such as _for_, _while_, and _loop_. Loops are typically used in processes to execute repetitive actions on signals or variables.

Example (For loop):
```vhdl
for i in 0 to 3 loop
  -- Perform some action for each iteration
end loop;
```

**Sequential Execution**: VHDL follows a specific execution order within processes. Statements inside a process are executed one after the other, making the order of operations crucial.

## Data Types and Operators

**Data Types**: VHDL has several built-in data types, including _bit_, _integer_, _std_logic_, and _std_logic_vector_. Each data type has specific uses depending on the application.

Example:
```vhdl
signal A : bit;  -- Single binary value
signal B : std_logic_vector(3 downto 0); -- 4-bit vector
```

**Operators**: VHDL supports a wide range of operators for performing logical, arithmetic, and relational operations.

Examples:
- Arithmetic operators: `+`, `-`, `*`, `/`
- Logical operators: `and`, `or`, `not`
- Relational operators: `=`, `/=`, `<`, `>`

## Types of Assignments: Blocking vs. Non-blocking

**Blocking Assignment**: In VHDL, blocking assignments are those that happen immediately and block further execution until they complete. The behavior is similar to how C programming handles assignments.

Example:
```vhdl
A := B;
```

**Non-blocking Assignment**: Non-blocking assignments allow other statements to execute before the value of a signal or variable is updated. This is typically used for concurrent signal assignments in VHDL.

Example:
```vhdl
A <= B;
```

## Component Instantiation

**Component Instantiation**: In VHDL, you can instantiate other entities (components) inside your design. Components represent smaller sub-modules that you design and then integrate into larger designs.

Example:
```vhdl
U1: AND_Gate port map (A => A_in, B => B_in, C => C_out);
```

----

These fundamental VHDL concepts provide the foundation for effectively designing and simulating digital circuits. 
