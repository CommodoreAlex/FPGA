# Table of Contents

1. [Choosing a Xilinx FPGA Development Board](#1-choosing-a-xilinx-fpga-development-board)
2. [Installing Vivado Design Suite](#2-installing-vivado-design-suite)
    - [Install Vivado](#step-2-install-vivado)
    - [Licensing](#step-3-licensing)
3. [Connecting Your FPGA Board](#3-connecting-your-fpga-board)
4. [Writing Your First VHDL Program](#4-writing-your-first-vhdl-program)
    - [Create a New Project](#step-1-create-a-new-project)
    - [Add VHDL Source File](#step-2-add-vhdl-source-file)
    - [Write VHDL Code](#step-3-write-vhdl-code)
5. [Simulating VHDL Code](#5-simulating-vhdl-code)
    - [Create a Testbench](#step-1-create-a-testbench)
    - [Run the Simulation](#step-2-run-the-simulation)
6. [Programming the FPGA](#6-programming-the-fpga)
    - [Generate Bitstream](#step-1-generate-bitstream)
    - [Program the FPGA](#step-2-program-the-fpga)
7. [Conclusion](#conclusion)

---

# Setting Up the Development Environment for Vivado with Xilinx FPGAs

In this section, you’ll learn how to set up the development environment for working with Xilinx FPGAs using Vivado, a powerful tool for FPGA design. Follow the steps below to get everything ready for your FPGA design projects.

## **1. Choosing a Xilinx FPGA Development Board**

You’ll need a Xilinx FPGA development board. For beginners, I recommend the **Basys 3 Artix-7 FPGA Trainer Board**, which is a great platform for learning FPGA design. You can find it here:

- [Basys 3 Artix-7 FPGA Trainer Board on Amazon](https://www.amazon.com/dp/B00NUE1WOG?ref=ppx_yo2ov_dt_b_fed_asin_title)

Something else to consider with your work on this FPGA, would be to invest in a case like: 

## **2. Installing Vivado Design Suite**

Vivado is the official development environment from Xilinx for synthesizing, simulating, and programming FPGAs. Follow these steps to install Vivado:

1. Go to the [Xilinx Vivado Design Suite](https://www.xilinx.com/support/download.html) page.
2. Download the version that suits your FPGA development board. The **Vivado WebPACK** version is free and works for most beginner projects.

### **Step 2: Install Vivado**

1. Download the installer for your operating system.
2. Run the installer and follow the on-screen instructions. Select **Vivado WebPACK Edition** if using the free version.
3. The installation may take some time, depending on your system.

![[Pasted image 20250302162952.png]]

### **Step 3: Licensing**

If using the **WebPACK** edition, no additional license is needed. For other editions, follow Xilinx’s instructions to obtain a license.

## **3. Connecting Your FPGA Board**

After installing Vivado, connect your Xilinx FPGA board to your computer using the appropriate **USB cable**:

- **Micro-USB cable**: For power and communication (used for many Xilinx FPGA boards, including the Basys 3).
- **USB-to-JTAG cable**: Some boards may require a dedicated programming cable, such as a **Digilent USB-JTAG** cable, for programming and debugging the FPGA.

Make sure to use the right cable for both **programming** and **powering** the FPGA. Power the FPGA board with the provided power supply, and Vivado should detect the board automatically. If not, you may need to install USB drivers.

## **4. Writing Your First VHDL Program**

Now that your environment is set up, you can write your first VHDL program.

### **Step 1: Create a New Project**

1. Open Vivado and create a new project.
2. Choose **RTL Project** and proceed with the setup, specifying the project name and location.
3. Add your sources and constraints later.

### **Step 2: Add VHDL Source File**

1. Under the **Project Manager**, go to the **Sources** tab.
2. Right-click in the **Design Sources** pane and select **New VHDL Module**.
3. Name the file (e.g., `Blink_LED`) and click **OK**.

### **Step 3: Write VHDL Code**

Here’s a simple VHDL code example to blink an LED:
```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity Blink_LED is
    Port ( CLK : in STD_LOGIC;
           LED : out STD_LOGIC);
end Blink_LED;

architecture Behavioral of Blink_LED is
begin
    process(CLK)
    begin
        if rising_edge(CLK) then
            LED <= not LED; -- Toggle LED on every clock edge
        end if;
    end process;
end Behavioral;
```

## **5. Simulating VHDL Code**

Vivado includes simulation tools for testing your VHDL code before programming the FPGA.

### **Step 1: Create a Testbench**

A **testbench** provides stimulus and observes the output of your design. Here’s an example for the `Blink_LED` design:
```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity tb_Blink_LED is
end tb_Blink_LED;

architecture behavior of tb_Blink_LED is
    signal CLK : STD_LOGIC := '0';
    signal LED : STD_LOGIC;
begin
    UUT: entity work.Blink_LED
        port map (CLK => CLK, LED => LED);

    -- Clock generation
    process
    begin
        CLK <= not CLK after 10 ns; -- 50 MHz clock
        wait for 10 ns;
    end process;
end behavior;
```

### **Step 2: Run the Simulation**

1. In Vivado, go to **Flow Navigator** > **Simulation** > **Run Simulation**.
2. Choose **Run Behavioral Simulation** to test the design and observe the waveform.

## **6. Programming the FPGA**

After simulating the design, you can program the FPGA.

### **Step 1: Generate Bitstream**

1. In Vivado, go to **Flow Navigator** and click **Generate Bitstream**.
2. This will compile your design into a file that can be programmed onto the FPGA.

### **Step 2: Program the FPGA**

1. **Connect the FPGA board** to your computer using the appropriate cable:
    - **Micro-USB cable**: If your board uses this cable for both power and communication.
    - **USB-to-JTAG cable**: If your board requires a dedicated JTAG cable for programming and debugging.
2. Power the FPGA board using the provided power supply.
3. In Vivado, open **Hardware Manager** and click **Program Device**.
4. Select the FPGA device and load the generated bitstream.
5. The FPGA will now be programmed, and you should see the LED blink.

---

## Conclusion

You’ve now set up Vivado and your Xilinx FPGA environment! You can start creating more complex VHDL designs, simulate them, and implement them on the FPGA hardware. This setup lays the foundation for diving into FPGA development and embedded systems applications.
