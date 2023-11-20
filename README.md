# 16 bit MIPS Processor Verification using UVM
The MIPS processor design represents a fundamental milestone in the evolution of contemporary processor architectures, integrating key concepts like Pipelining, Data Dependency handling, and Forwarding to enhance processing capabilities and speed.


## Project Overview
This project aims to verify the functionality of a five-stage pipelined MIPS processor. The processor features 16 instructions with 49 variants, 5 pipeline stages, and an integrated hazard unit. 
The verification process employs advanced techniques, including Constrained Random Verification for assessing individual instruction functionality and Assertion Based Verification to validate data dependencies managed by the hazard unit. 
The entire verification process is implemented using the System Verilog Unified Verification Methodology (UVM).


## Design Specifications
The MIPS processor under scrutiny is a 16-bit, five-staged pipelined processor, providing 16 instructions with 49 variations. The verification process follows a two-step approach: 
first, the sequential verification of individual blocks within each stage, and second, the validation of the overall functionality of the design. 
Automation is leveraged to streamline verification, utilizing a single testing system with interfaces to internal and main input-output blocks. 
This approach ensures a highly automated, rapid, and accurate verification process.


## Design Under Test
The design comprises the following five stages:
1. **Instruction Fetch:** This stage houses all instructions slated for execution and includes a program counter to indicate the current instruction in progress.

2. **Decode Stage:** Responsible for simplifying instructions and coordinating processor operations through various control signals, this stage also accommodates general-purpose registers for runtime data storage.

3. **Execution Stage:** This stage executes arithmetic and logical instructions.

4. **Memory Access:** Featuring the memory unit, this stage serves as the primary data memory storage for the processor.

5. **Write Back:** The final stage involves writing data back to the appropriate register in the register file after completing operations.


## Instruction set
The MIPS processor design encompasses a diverse instruction set, offering a range of operations to facilitate varied computational tasks. The instructions are categorized based on their functionality, and each instruction specifies its type, opcode, 
control signals, reconfiguration, destination register, source registers, and memory operations. Below is a brief overview of the MIPS processor's instruction set:

| Instruction   | Function           | Opcode | Ctrl | Reconfig | Dest | Scr1 | Scr2/Mem  |
| -------------- | ------------------  | ------ | ---- | ---------| ---- | ---- | --------- |
| Addition       | C=A+B               | 0      | X    | 11       | [dest] | [scr1] | [scr2] |
|                | z=x+y               | 0      | X    | 10       | [dest] | [scr1] | [scr2] |
|                | c=a+b               | 0      | X    | 1        | [dest] | [scr1] | [scr2] |
| Subtraction    | C=A-B               | 1      | X    | 11       | [dest] | [scr1] | [scr2] |
|                | z=x-y               | 1      | X    | 10       | [dest] | [scr1] | [scr2] |
|                | c=a-b               | 1      | X    | 1        | [dest] | [scr1] | [scr2] |
| Increment      | C=A+1               | 11     | X    | 11       | [dest] | [scr1] | [scr2] |
|                | z=x+1               | 11     | X    | 10       | [dest] | [scr1] | [scr2] |
|                | c=a+1               | 11     | X    | 1        | [dest] | [scr1] | [scr2] |
| Decrement      | C=A-1               | 10     | X    | 11       | [dest] | [scr1] | [scr2] |
|                | z=x-1               | 10     | X    | 10       | [dest] | [scr1] | [scr2] |
|                | c=a-1               | 10     | X    | 1        | [dest] | [scr1] | [scr2] |
| And/Nand       | C = A&B             | 100    | 0    | 11       | [dest] | [scr1] | [scr2] |
|                | z = x&y             | 100    | 0    | 10       | [dest] | [scr1] | [scr2] |
|                | c = a&b             | 100    | 0    | 1        | [dest] | [scr1] | [scr2] |
| !C = A&B       | 100    | 1    | 11    | [dest] | [scr1] | [scr2] |
|                | !z = x&y            | 100    | 1    | 10       | [dest] | [scr1] | [scr2] |
|                | !c = a&b            | 100    | 1    | 1        | [dest] | [scr1] | [scr2] |
| Or/Nor         | C=A|B               | 101    | 0    | 11       | [dest] | [scr1] | [scr2] |
|                | z = x|y             | 101    | 0    | 10       | [dest] | [scr1] | [scr2] |
|                | c = a|b             | 101    | 0    | 1        | [dest] | [scr1] | [scr2] |
| C = A|B        | 101    | 1    | 11   | [dest] | [scr1] | [scr2] |
|                | z = x|y             | 101    | 1    | 10       | [dest] | [scr1] | [scr2] |
|                | c = a|b             | 101    | 1    | 1        | [dest] | [scr1] | [scr2] |
| Exor/Exnor     | C=AˆB               | 110    | 0    | 11       | [dest] | [scr1] | [scr2] |
|                | z = xy              | 110    | 0    | 10       | [dest] | [scr1] | [scr2] |
|                | c = ab              | 110    | 0    | 1        | [dest] | [scr1] | [scr2] |
| C = A B        | 110    | 1    | 11   | [dest] | [scr1] | [scr2] |
|                | z = x y             | 110    | 1    | 10       | [dest] | [scr1] | [scr2] |
|                | c = a b             | 110    | 1    | 1        | [dest] | [scr1] | [scr2] |
| Buffer         | C=A                 | 111    | 0    | 11       | [dest] | [scr1] | XXX      |
|                | z=x                 | 111    | 0    | 10       | [dest] | [scr1] | XXX      |
|                | c=a                 | 111    | 0    | 1        | [dest] | [scr1] | XXX      |
| Inversion      | C =!A               | 111    | 1    | 11       | [dest] | [scr1] | XXX      |
|                | z =!x               | 111    | 1    | 10       | [dest] | [scr1] | XXX      |
|                | c =!a               | 111    | 1    | 1        | [dest] | [scr1] | XXX      |
| Multiplication | C=a*b               | 1000   | X    | 1        | [dest] | [scr1] | [scr2] |
|                | C = x * y           | 1000   | X    | 10       | [dest] | [scr1] | [scr2] |
| Shift          | C = leftshift(A)    | 1100   | 0    | 11       | [dest] | [scr1] | [shift val] |
|                | C = rightshift(A)   | 1100   | 1    | 11       | [dest] | [scr1] | [shift val] |
| Load           | C=mem               | 1010   | X    | 11       | [dest] | XXX   | [mem]    |
| Store          | Mem=A               | 1011   | X    | 11       | XXX    | [scr1] | [mem]    |
| Move           | C=A                 | 1001   | X    | 11       | [dest] | [scr1] | XXX      |
|                | z=x                 | 1001   | X    | 10       | [dest] | [scr1] | XXX      |
|                | c=a                 | 1001   | X    | 1        | [dest] | [scr1] | XXX      |
| Immediate      | z=8bit data         | 1001   | 1    | 10       | "{X, {8-bit data}}"       |
|                | c=8bit data         | 1001   | 1    | 1        | "{X, {8-bit data}}"       |
| JMP            | Jmp unconditional   | 1101   | X    | XX       | "{XXXXXX, {3-bit disp}}" |
| NOP            | End (waste a cycle) | 1110   | X    | XX       | XXX XXX XXX              |
| EDP            | EOP (End of Operation| 1111   | X    | XX       | XXX XXX XXX              |


## Inputs and Outputs
The table shows the input-output signals of the design. It must be noted that the processor is a closed system, ie. there are only memory units holding the data both in input and output. Since it is a closed system there is no concrete input or output
signals. In the given table we show only the internal signals which carry the data most relevant to our testing.

| Name             | Direction | Width | Description                                      |
|------------------|-----------|-------|--------------------------------------------------|
| inst_in          | input     | 15:0  | The instruction fed to the pipeline to decode and operate on. |
| reg_write        | output    | 15:0  | Write back value that can be written to the register for particular conditions. |
| wr_reg_address   | output    | 2:0   | Register address where the value is to be written. |
| wr_reg_enable    | output    | 1:0   | Signal asserted true when writing to the register file. |
| jump             | output    | 2:0   | Value of increment in pc.                         |
| jump_enable      | output    | 1     | States the condition when pc+jump needs to be taken. |
| mem_write        | output    | 15:0  | Write value that can be written to the Data Memory for particular conditions. |
| wr_mem_addr      | output    | 2:0   | Address where the value can be written in the memory. |
| mem_enable       | output    | 1     | Value is written only when this signal is asserted true. |
| reg_read         | output    | 15:0  | Sources read from the register file and moved to the execute stage. |
| rd_reg_enable    | output    | 1:0   | MSB or LSB that needs to be read from a register address. |
| rd_reg_address   | output    | 2:0   | Address from where data is read from.             |
| rd_mem_addr      | output    | 2:0   | Memory address from where data is read from.      |


## Verification functionality and test cases
The verification plan outlined in the given context involves a systematic approach to validate the MIPS processor design. The plan is divided into two main parts:

1. **Data Independent Instruction Testing:**
   - **Objective:** Verify individual instructions for functionality.
   - **Method:** Constrained Random Verification (CRV).
   - **Process:**
     - Test each instruction separately.
     - Utilize CRV to cover various scenarios for each instruction.
     - Assess the functionality by checking if the output aligns with expected results.
   - **Challenges and Considerations:**
     - Testing individual instructions may not capture data dependencies.
     - Automation and efficiency are key to streamline testing.

2. **Data Dependent Instruction Testing:**
   - **Objective:** Verify instructions with data dependencies.
   - **Method:** Assertion Based Verification (ABV).
   - **Process:**
     - Identify instructions with data dependencies (e.g., MOV, LOAD, STORE).
     - Design test cases to check proper handling of data dependencies.
     - Use assertions to validate correct execution of data-dependent instructions.
   - **Challenges and Considerations:**
     - Data dependencies require careful handling.
     - ABV ensures specific scenarios are tested to validate the hazard unit's functionality.


| Functionality to be tested        | Type of Coverage           | Test Description                                               |
|-----------------------------------|---------------------------|-----------------------------------------------------------------|
| Addition                          | Constrained Random Verification | Test the working of this instruction by checking if the output has the sum of the input operands. |
| Subtraction                       | Constrained Random Verification | Test the working of this instruction by checking if the output has the difference of the input operands. |
| Increment                         | Constrained Random Verification | Test the working of this instruction by checking if the output has the value one more than the input operand's value. |
| Decrement                         | Constrained Random Verification | Test the working of this instruction by checking if the output has the value one less than the input operand's value. |
| AND & NAND Operation              | Constrained Random Verification | Test the working of this instruction by checking if the output is AND or NAND of the input operands. |
| OR & NOR Operation                | Constrained Random Verification | Test the working of this instruction by checking if the output is OR or NOR of the input operands. |
| XOR & XNOR Operation              | Constrained Random Verification | Test the working of this instruction by checking if the output is XOR or XNOR of the input operands. |
| Multiplication                    | Constrained Random Verification | Randomly call this instruction on two 8-bit numbers and check if the output is the product of the inputs. |
| SHIFT Left and Right Operation     | Constrained Random Verification | Call the instruction on random input data and check if the data gets shifted by one bit. |
| LOAD Data                         | Constrained Random Verification | Choose a random location in the memory, load data into the register file, and check if the register file contains that data. |
| STORE Data                        | Constrained Random Verification | Choose a random source register, store data into the memory, and check if the memory contains that data. |
| MOV Instruction                   | Constrained Random Verification | Choose random source and destination registers, move data, and check if the data has actually moved. |
| JUMP Instruction                  | Constrained Random Verification | Choose a random number for incrementing the instruction counter and check if the instruction counter has actually been incremented. |
| MOVI Instruction                  | Assertion Based Verification   | Test all MOVI instructions used to initialize the processor by asserting each operation once. |
| NOP Instruction                   | Assertion Based Verification   | Test all NOP instructions used to initialize the processor by asserting each operation once. |
| EOP Instruction                   | Assertion Based Verification   | Test the EOP instruction's working by calling the instruction in a loop for more than 10 times and asserting the EOP to be performed each time. |


## Coverage Bins 
- **Objective:** Validate complete functional behavior of the processor.
- **Bins:**
  - Assign coverage bins for each instruction type.
  - Bins cover the opcode ranges and specific configurations for different instructions.
  - Track coverage to ensure all aspects of the processor design are thoroughly tested.

| Instruction             | Signal Coverage Bins        |
|-------------------------|-----------------------------|
| Addition Instruction    | [16'h0000 : 16'h0FFF]      |
| Subtraction Instruction | [16'h1000 : 16'h1FFF]      |
| Increment Instruction    | [16'h3000 : 16'h3FFF]      |
| Decrement Instruction    | [16'h2000 : 16'h2FFF]      |
| AND/NAND Instruction     | [16'h4000 : 16'h4FFF]      |
| OR/NOR Instruction        | [16'h5000 : 16'h5FFF]      |
| EXOR/EXNOR Instruction    | [16'h6000 : 16'h6FFF]      |
| Buffer/Inversion Instruction | [16'h7000 : 16'h7FFF]   |
| Multiplication Instruction | [16'h8000 : 16'h8FFF]     |
| Shift Left/Right Instruction | [16'hC000 : 16'hCFFF]   |
| Load Instruction          | [16'hA000 : 16'hAFFF]     |
| Store Instruction         | [16'hB000 : 16'hBFFF]     |
| MOV/MOVI Instruction      | [16'h9000 : 16'h9FFF]     |
| Jump Instruction          | [16'hD000 : 16'hDFFF]     |
| NOP Instruction           | [16'hE000 : 16'hEFFF]     |


## Layered testbench
The layered testbench in the context of the MIPS processor design employs the Universal Verification Methodology (UVM) framework, utilizing System Verilog. It follows an organized and modular structure to facilitate comprehensive testing.

1. **Processor Testbench Package:**
   - **Description:** Acts as a package file that includes all other test modules, consolidating the entire testbench for easy compilation and execution.
   - **Purpose:** Simplifies the organization and management of the testbench components.

2. **Processor Test:**
   - **Description:** Extends the `uvm_test` class from UVM.
   - **Functionality:**
     - Initiates and runs tests on the Design Under Test (DUT).
     - Creates the test environment and transaction modules.
     - Initiates testing by triggering the sequencer.

3. **Processor Top:**
   - **Description:** Responsible for instantiating the DUT and Interface.
   - **Functionality:**
     - Initiates various tests within the testbench.
     - Manages the top-level connections and components.

4. **Processor Environment:**
   - **Description:** Sets up the test environment, including the agent, monitor, subscriber, and scoreboard.
   - **Functionality:**
     - Instantiates various units during the build phase.
     - Establishes connections between different components during the connect phase.

5. **Processor Agent:**
   - **Description:** Contains the driver and sequencer.
   - **Functionality:**
     - Handles the instantiation of the driver and sequencer.
     - Manages the connections between the sequencer and the driver.

6. **Processor Interface:**
   - **Description:** Manages the connection between the DUT and the testbench, specifically the Monitor and Driver.
   - **Functionality:**
     - Defines the signals being connected and their types between the DUT and testbench components.

7. **Processor Monitor:**
   - **Description:** Monitors and records inputs from the DUT.
   - **Functionality:**
     - Records signals for every 20 instruction changes.
     - Sends recorded signals to the scoreboard for further processing.

8. **Processor Driver:**
   - **Description:** Utilizes two drivers for Constrained Random Verification (CRV) and Assertion Based Verification (ABV) tests.
   - **Functionality:**
     - Drives signals to the DUT for testing.
     - Sends the same signals to the scoreboard for comparison.

9. **Processor Scoreboard:**
   - **Description:** Performs signal checking and reporting of verification.
   - **Functionality:**
     - Compares input and output signals.
     - Reports the results of the verification process.

10. **Processor Sequence:**
    - **Description:** Represents the base transaction object shared between the sequencer, driver, and scoreboard.
    - **Functionality:**
      - Holds instruction and output-related data.

11. **Processor Subscriber:**
    - **Description:** Keeps track of line coverage and group coverage.
    - **Functionality:**
      - Takes input from the monitor.
      - Generates a report with statistics on code coverage and group coverage.

The layered testbench structure ensures a modular and systematic approach to verification, enhancing maintainability and ease of testing different aspects of the MIPS processor design.

## Conclusion 
The project marks a significant milestone in comprehending and testing the 16-bit, five-staged pipelined MIPS processor design. Utilizing a structured verification plan, the processor's instructions underwent rigorous testing, 
showcasing functionality and highlighting the importance of timing in a pipelined architecture. The project's division into basic instruction testing, unit testing, and comprehensive instruction testing provides a clear roadmap for future endeavors. 
Preliminary results affirm the proper functioning of the design, and the adoption of Universal Verification Methodology (UVM) is set to streamline testing processes. The project lays a solid foundation for further exploration, understanding, 
and refinement of the MIPS processor design.
