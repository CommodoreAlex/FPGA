### Hands-on FPGA Projects

This section provides beginner-friendly FPGA projects to reinforce VHDL concepts and practical FPGA development. Each project includes a description, required components, and VHDL code examples.

---

## 1. Blinking LED (Hello World of FPGA)

### Description

A simple project that makes an onboard LED blink at a defined interval using a clock divider. The clock signal is divided using a counter to create a slower frequency for the LED blink.

### Components

- **FPGA Board** (e.g., Basys 3, DE10-Lite)
- **Onboard LED** (most FPGA boards have this built-in)

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity Blinking_LED is
    Port ( clk : in STD_LOGIC;
           led : out STD_LOGIC);
end Blinking_LED;

architecture Behavioral of Blinking_LED is
    signal counter: INTEGER := 0;
    signal led_state: STD_LOGIC := '0';
begin
    process(clk)
    begin
        if rising_edge(clk) then
            counter <= counter + 1;
            if counter = 50000000 then -- Adjust for 100 MHz clock speed
                led_state <= not led_state;  -- Toggle the LED state
                counter <= 0;  -- Reset the counter
            end if;
        end if;
    end process;
    led <= led_state;
end Behavioral;
```

### Additional Explanation:

_"In this project, we use a counter to divide the FPGA's clock signal. By incrementing the counter on every rising clock edge, we can control the blink interval. Adjust the counter threshold based on the FPGA clock speed to change the blink rate."_

---

## 2. Button Input and Debouncing

### Description

Detects button presses while eliminating false triggers due to mechanical bouncing. The input signal is sampled and filtered to ensure that only stable button presses are detected.

### Components

- **FPGA Board**
- **Push Button** (if not available on the board, an external button is needed)
- **LED** (for output)

### VHDL Code Snippet
```vhdl
process(clk)
begin
    if rising_edge(clk) then
        debounce_reg <= debounce_reg(0) & button;  -- Shift the button signal into the register
        if debounce_reg = "11" then  -- When both bits are '1', the button is stable
            button_stable <= '1';  -- Button press detected
        else
            button_stable <= '0';  -- Button not stable
        end if;
    end if;
end process;
```

### Additional Explanation:

_"In this project, we implement a simple debouncing technique by shifting the button input through a 2-bit register. When both bits are '1', it indicates that the button is stable and not bouncing, meaning a valid press has occurred."_

---

## 3. Simple State Machine

### Description

Implements a finite state machine (FSM) to control a sequential process, transitioning between different states based on input signals. This example FSM can represent a process like task execution or event sequencing.

### Example States:

1. **IDLE** – Wait for an input.
2. **PROCESS** – Execute a task.
3. **DONE** – Signal completion.

### VHDL Code Snippet
```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity Simple_FSM is
    Port ( clk : in STD_LOGIC;
           reset : in STD_LOGIC;
           signal_in : in STD_LOGIC;
           signal_out : out STD_LOGIC);
end Simple_FSM;

architecture Behavioral of Simple_FSM is
    type state_type is (IDLE, PROCESS, DONE);  -- Define FSM states
    signal state, next_state : state_type := IDLE;
begin
    process(clk, reset)
    begin
        if reset = '1' then
            state <= IDLE;  -- Reset to initial state
        elsif rising_edge(clk) then
            state <= next_state;  -- Update state on clock edge
        end if;
    end process;

    process(state, signal_in)
    begin
        case state is
            when IDLE =>
                if signal_in = '1' then
                    next_state <= PROCESS;
                else
                    next_state <= IDLE;
                end if;
            when PROCESS =>
                next_state <= DONE;
            when DONE =>
                next_state <= IDLE;
        end case;
    end process;

    -- Output logic
    signal_out <= '1' when state = DONE else '0';
end Behavioral;
```

---
### Additional Explanation:

_"This project implements a simple FSM with three states: IDLE, PROCESS, and DONE. The FSM transitions from one state to another based on the input signal and clock. The output `signal_out` is activated when the FSM reaches the DONE state."_

