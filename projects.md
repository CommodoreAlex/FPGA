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

### Additional Explanation:

_"This project implements a simple FSM with three states: IDLE, PROCESS, and DONE. The FSM transitions from one state to another based on the input signal and clock. The output signal_out is activated when the FSM reaches the DONE state."_

---

## 4. Cybersecurity and Technology Resiliency FPGA Project

### Description

This project demonstrates how FPGA technology can enhance cybersecurity and system resiliency. The design implements a hardware-based cryptographic function to authenticate and secure communications, protecting against unauthorized access and tampering.

### Components

1. **Secure Key Storage Module (Microchip ATECC608A)**:
    - Around **$5 to $10** per unit.
2. **Cryptographic Accelerator (AES, SHA)**:
    - No additional cost since the ATECC608A includes AES and SHA functionality.
3. **I2C Interface Module**:
    - Typically around **$5 to $15**, depending on the module.
4. **Connecting Wires (Jumper wires)**:
    - A basic set of jumper wires (10-20 pieces) costs around **$5 to $10**.

Total Estimated Cost for This Project: **$15 to $35** (depending on specific choices and quantity).

### VHDL Code Snippet

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity AES_Encryptor is
    Port ( clk : in STD_LOGIC;
           reset : in STD_LOGIC;
           data_in : in STD_LOGIC_VECTOR(127 downto 0);
           key : in STD_LOGIC_VECTOR(127 downto 0);
           data_out : out STD_LOGIC_VECTOR(127 downto 0));
end AES_Encryptor;

architecture Behavioral of AES_Encryptor is
begin
    process(clk, reset)
    begin
        if reset = '1' then
            data_out <= (others => '0');
        elsif rising_edge(clk) then
            -- AES encryption logic (simplified for demonstration purposes)
            data_out <= data_in XOR key;
        end if;
    end process;
end Behavioral;
```

### Additional Explanation:

_"This FPGA-based security project implements a basic AES encryption function. The input data is XORed with the key to simulate encryption (for demonstration purposes). In real-world applications, a full AES implementation would use S-Boxes, MixColumns, and key expansion. FPGA-based encryption enhances security by providing a dedicated, tamper-resistant processing environment, making it harder for attackers to extract cryptographic keys."_

---

