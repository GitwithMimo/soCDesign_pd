## Digital VLSI SoC Design and Planning

## Contents
[ASIC Design Flow](#asic-design-flow)

[DAY-1: Overview of open source EDA OpenLANE and Sky130PDK](#day-1-overview-of-open-source-eda-openlane-and-sky130pdk)

[DAY-2: Good Floorplan vs bad Floorplan and Introduction to library cells](#day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)

## ASIC Design Flow
ASIC (Application-Specific Integrated Circuit) design flow refers to the series of steps involved in designing a custom chip tailored for a specific application. Unlike general-purpose ICs, ASICs are optimized for particular tasks, making them highly efficient for their intended purpose. The design flow is a structured process that ensures the final chip meets all functional, performance, and manufacturability requirements.

Major Steps in the ASIC Design Flow
1. Specification:
2. Design Entry (RTL Design)
3. Functional Verification
4. Synthesis
5. Design for Testability (DFT)
6. Static Timing Analysis (STA)
7. Floorplanning
8. Placement and Routing
9. Power Analysis
10. Physical Verification
11. RC Extraction and Post-Layout STA
12. Signoff and GDSII Generation
13. Fabrication
14. Testing and Packaging




## DAY-1: Overview of open source EDA OpenLANE and Sky130PDK

#### OpenLANE
OpenLANE is an open-source electronic design automation (EDA) tool that provides a complete flow for the digital integrated circuit (IC) design from RTL (Register-Transfer Level) to GDSII (Graphic Data System II, a standard file format for IC layout). It is designed to automate the process of physical design, which includes various steps such as synthesis, floorplanning, placement, routing, and verification.

some information about OpenLANE:

1. Open Source: OpenLANE is released under the Apache License, Version 2.0, which allows for free use, modification, and distribution of the software.

2. Integrated Flow: It integrates several open-source tools to provide a complete physical design flow. Some of the tools used include Yosys for synthesis, OpenSTA for static timing analysis, Magic for layout, Netgen for LVS (Layout Versus Schematic), and Fault for DRC (Design Rule Checking).

3. Technology Independence: OpenLANE is designed to be technology-agnostic, meaning it can be adapted to different semiconductor manufacturing processes with the appropriate technology files.

4. Automation: The tool aims to automate the physical design process, reducing the need for manual intervention and expertise in each step of the flow.

5. Community Support: OpenLANE is backed by a community of developers and users who contribute to its development and provide support through forums and documentation.

6. Cross-Platform: It is designed to run on Linux-based systems, which is the standard operating environment for most EDA tools.

7. Documentation and Tutorials: OpenLANE comes with comprehensive documentation and tutorials to help users get started with the tool and understand its capabilities.

8. Contributions: The project welcomes contributions from the community, including bug fixes, new features, and improvements to the existing flow.

9. Applications: OpenLANE can be used for educational purposes, research, and even for small-scale commercial projects. It is particularly useful for academics and hobbyists who may not have access to expensive commercial EDA tools.

10. Ecosystem: OpenLANE is part of a larger ecosystem of open-source EDA tools and is often used in conjunction with other tools like OpenROAD for detailed routing and OpenPhySyn for physical synthesis.



#### OpenLANE ASIC Flow
1. Design RTL:
   Input: The flow starts with the RTL (Register Transfer Level) code, which is the high-level design description of the integrated circuit.
2. RTL Synthesis (Yosys):
   Yosys: Yosys is an open-source tool that converts the RTL design (written in Verilog) into a gate-level netlist.
3. Static Timing Analysis (STA - OpenSTA):
   OpenSTA: This tool performs static timing analysis on the synthesized netlist to check if the design meets timing requirements.
4. DFT (Design for Testability) - Fault:
   Fault Tool: DFT techniques are applied to the design to make it easier to test and debug. This step ensures that manufacturing defects can be detected during testing.
5. OpenROAD App (Physical Design):
   Floorplanning: The layout of the major blocks in the design is planned, setting up the physical boundaries and relative placement of components.
   Placement: The placement of standard cells (basic building blocks of the chip) is optimized.
   Clock Tree Synthesis (CTS): A clock distribution network is generated to ensure the clock signal reaches all parts of the circuit simultaneously.
   Optimization: Further adjustments are made to improve performance, reduce power consumption, or fix timing violations.
   Global Routing: High-level routing paths are planned before detailed routing.
6. Fake Antenna Diodes Insertion Script:
   Insertion of Antenna Diodes: During the routing process, fake antenna diodes are inserted to protect transistors from damage due to charge accumulation on metal lines during manufacturing.
7. Detailed Routing (TritonRoute):
   TritonRoute: A detailed routing tool that defines the exact paths of wires connecting different components of the design.
8. Logical Equivalence Check (LEC - Yosys):
   Yosys: After routing, the design is checked to ensure that the netlist is logically equivalent to the original RTL, verifying that no functional changes have occurred.
9. STA (OpenSTA):
   Second Pass STA: Another round of static timing analysis is performed on the design, now considering the real delays due to the physical layout (post-layout STA).
10. Physical Verification (Magic & Netgen):
   Magic: A layout tool used for physical verification, such as DRC (Design Rule Check) and LVS (Layout vs. Schematic) checks.
   Netgen: A tool used for verifying that the physical layout corresponds to the logical design.
11. GDSII Streaming (Magic):
   GDSII Output: Finally, the verified design is streamed out as a GDSII file, which is the standard format used to send the design to fabrication.
14. SKY130 PDK:
   PDK: The process design kit (PDK) used in this flow is SKY130, an open-source 130nm technology node provided by SkyWater Technology. This PDK contains the models, design rules, and libraries necessary to design and fabricate the IC.



#### PDK
A Process Design Kit (PDK) is a set of technical design files and documentation that provides the necessary information for designing integrated circuits (ICs) for a specific semiconductor fabrication process. The PDK is essential for semiconductor design because it contains the rules, models, and parameters that ensure the designed circuit can be manufactured successfully on a particular fabrication line or "process."

Here are some key components and aspects of a PDK:

a. Design Rules: These are the geometric constraints that define how small the features can be, how close they can be to each other, and other layout considerations to ensure that the design can be manufactured without defects.

b. Device Models: These are mathematical models that represent the electrical behavior of transistors and other components. They are used in simulation to predict how the circuit will perform before it is fabricated.

c. Layout Libraries: These include standard cells, IP blocks, and other pre-designed components that can be used as building blocks in the design process. They are typically provided in a format that can be used with electronic design automation (EDA) tools.

d. Technology Files: These files contain information about the fabrication process, such as layer stacks, materials, and process-specific parameters. They are used by EDA tools to ensure that the design adheres to the specific requirements of the fabrication process.

e. Simulation Models: In addition to device models, PDKs may include other simulation models for parasitic extraction, reliability analysis, and other aspects of circuit performance.

f. Documentation: PDKs come with extensive documentation that explains how to use the kit, the meaning of various parameters, and the best practices for design with the specific process.

g. Characterization Data: This includes measured data from test structures that have been fabricated and tested on the actual process. This data is used to validate the accuracy of the models and design rules.

h. Support and Updates: PDKs are often accompanied by support from the foundry or IP provider, including updates to reflect process improvements or corrections.

#### some of my LINUX code snippets from day-1 class

![Screenshot from 2024-08-22 23-08-43](https://github.com/user-attachments/assets/ad6bd3ff-4f3f-4402-8ff5-83761b2be1c6)
![Screenshot from 2024-08-22 23-07-28](https://github.com/user-attachments/assets/5bad31c5-0e2f-469e-ab6f-574822d97856)
![Screenshot from 2024-08-22 22-49-36](https://github.com/user-attachments/assets/b3f3ae1c-d77a-4355-94fc-6f2bc0b6022f)
![Screenshot from 2024-08-22 22-28-26](https://github.com/user-attachments/assets/ccb5afc3-e9a6-48f4-a468-197e8089d7e2)
![Screenshot from 2024-08-22 22-27-12](https://github.com/user-attachments/assets/5a2c5ce1-e0c2-4bfb-847c-ad499b16d89a)
![Screenshot from 2024-08-22 19-45-35](https://github.com/user-attachments/assets/af2ce2da-acbf-410a-81ea-20b8f3538705)

### snippets from the SYNTHESIS task of picorv32a 
#### here you can see: total no. of cells and total no. D FlipFlop used & the chip area for module

![Screenshot from 2024-08-22 23-23-06](https://github.com/user-attachments/assets/f8841bc0-0fdb-456a-b3e5-be5eb849e277)
![Screenshot from 2024-08-22 23-20-18](https://github.com/user-attachments/assets/19b9e6fa-75a7-462e-bb1c-4a0ba72df3ce)
![Screenshot from 2024-08-22 23-20-53](https://github.com/user-attachments/assets/e13a8bbd-2485-45ca-abdd-35d86b9c97d8)

#### The Flop Ratio
flop ratio =(total no. of d flop realised) / (total no. cells)

           = 1613/14876
           
           = 0.10842
           
% ratio= 10.842




## DAY-2: Good Floorplan vs bad Floorplan and Introduction to library cells

### CONTENTS
# My Project

## Table of Contents

- [Utilization factor and Aspect ratio of a chip or die:](#utilization-factor-and-aspect-ratio-of-a-chip-or-die)
- [Concept of pre-placed cells and de-coupling capacitors:](#concept-of-pre-placed-cells-and-de-coupling-capacitors)
- [Hands-on LABs](#hands-on-LABs--)
- [Cell design and characterization flow:-](#cell-design-and-characterization-flow-)
- [Timing threshold and Propagation delay:-](#timing-threshold-and-propagation-delay-)

### Utilization factor and Aspect ratio of a chip or die:
#### Utilization Factor:
The utilization factor of a chip refers to the ratio of the area that is actually used for active devices (like transistors, capacitors, etc.) to the total area of the chip. It is a measure of how efficiently the available silicon real estate is being used. A higher utilization factor indicates that more of the chip's area is dedicated to functional components, which is generally desirable as it maximizes the performance or functionality per unit area.

##### Aspect Ratio:
The aspect ratio of a chip is the ratio of its length to its width. It is a geometric property that describes the shape of the chip. The aspect ratio is important for several reasons.

![106354493-338a5200-6318-11eb-93a0-80552f1135c8](https://github.com/user-attachments/assets/9858e158-b975-4f92-9b9b-f47b0e092d5d)

### Concept of pre-placed cells and de-coupling capacitors:

#### Pre-placed Cells:
Pre-placed cells are components or functional units within an IC or on a PCB that are placed at specific locations before the main placement and routing process begins. These cells are typically used for power management, signal integrity, or other critical functions that require precise positioning to meet design constraints. Pre-placed cells can include decoupling capacitors, power pads, voltage regulators, or other components that are essential for the proper functioning of the circuit.

#### Decoupling Capacitors:
Decoupling capacitors are small-value capacitors placed between the power supply rails (Vcc and Gnd) of an integrated circuit or on a PCB to provide a local source of energy that can supply transient current demands of the active devices. They help to "decouple" the local power supply from the rest of the circuit, ensuring that rapid changes in current demanded by one component do not affect the supply voltage of other components.

![figure-1-1](https://github.com/user-attachments/assets/385ed41e-7e34-4c44-a8b0-3799daa7a077)

![EndCapPlacement](https://github.com/user-attachments/assets/0b65cd60-04fd-4b7b-94af-f2bf97e828c4)

### Hands-on LABs:--

#### 1-Run_floorplan:

![Screenshot from 2024-08-24 15-59-11](https://github.com/user-attachments/assets/3f6738d0-0a08-4bd6-898f-dadecf5dd6ab)

![Screenshot from 2024-08-24 16-05-01](https://github.com/user-attachments/assets/ceb5ac07-f77f-4cf4-84f5-01c4160c6978)

#### 2-ioPlacer.log :

![Screenshot from 2024-08-24 16-06-18](https://github.com/user-attachments/assets/afb116d4-dcea-4185-bc04-e66adff29710)

![Screenshot from 2024-08-24 16-22-37](https://github.com/user-attachments/assets/e84e5c3c-b47c-4916-a6ae-36c519d6dd6c)

#### 3-Picorv32a floorplan def file:

![Screenshot from 2024-08-25 23-32-59](https://github.com/user-attachments/assets/dadd262d-a644-4423-834c-88cdc3cdb89b)

#### 4-magic tool tech file and the layout of design:

![Screenshot from 2024-08-25 23-45-18](https://github.com/user-attachments/assets/f2e3bf26-01f4-4108-a825-129cf5ddbde2)

![Screenshot from 2024-08-25 23-45-31](https://github.com/user-attachments/assets/6355bce6-e7bc-4f6b-97a4-df0432012766)

##### Input pins of layout and information about a pin in tkcon main using "What" command:

![Screenshot from 2024-08-26 00-01-09](https://github.com/user-attachments/assets/cd7f7df3-bd61-4ba9-9c0f-cec1a383bff6)

little zoomed in

![Screenshot from 2024-08-26 00-01-18](https://github.com/user-attachments/assets/7ea5090a-59cd-4a82-bd97-2d237b11f442)

![Screenshot from 2024-08-26 00-01-59](https://github.com/user-attachments/assets/9125b778-58c4-4d57-9458-f1b0a74e05b0)

![Screenshot from 2024-08-26 00-13-09](https://github.com/user-attachments/assets/365eeebf-3d8b-44c8-9102-6ee61cd235ae)

#### 5-Congestion aware placement using RePlace:

![Screenshot from 2024-08-26 01-09-48](https://github.com/user-attachments/assets/f0b33f46-165d-4bab-8151-5c79ffc6ece9)

![Screenshot from 2024-08-26 01-16-59](https://github.com/user-attachments/assets/03e959a9-61d5-41ae-b7be-b32217fe0c72)

![Screenshot from 2024-08-26 01-17-06](https://github.com/user-attachments/assets/9b120162-0598-46d4-b5b3-e188de85633b)

![Screenshot from 2024-08-26 01-20-35](https://github.com/user-attachments/assets/93fd7307-3503-44fe-b39d-f5250fdaf18e)


### Cell design and characterization flow:-
#### Cell design flow:
The cell design flow is a systematic process used to create and optimize individual cells within an integrated circuit (IC). These cells are basic building blocks, such as logic gates, flip-flops, or other functional units. The cell design flow typically involves the following steps:

 #### 1-Specification and Requirements:
        a. Define the cell's functionality and performance requirements (e.g., speed, power, area).
        b. Establish operating conditions and constraints.

 #### 2-Conceptual Design:
        a. Develop a high-level representation using hardware description languages (HDL) like Verilog or VHDL.
        b. Perform initial simulations to validate the concept.

 #### 3-Schematic Capture:
        a. Create a detailed schematic using electronic design automation (EDA) tools.
        b. Include all necessary components (transistors, resistors, capacitors, etc.).

 #### 4-Circuit Simulation:
        a. Simulate the cell using tools like SPICE to verify functionality and performance.
        b. Adjust the design as needed to meet specifications.

 #### 5-Layout Design:
        a. Translate the schematic into a physical layout, placing and routing components on the silicon substrate.
        b. Ensure adherence to design rules and constraints.

 #### 6-Layout Versus Schematic (LVS) Check:
        a. Compare the layout to the schematic to ensure they match.
        b. Fix any discrepancies.

 #### 7-Design Rule Check (DRC):
        a. Verify that the layout complies with fabrication design rules.
        b. Make necessary adjustments to pass the DRC.

 #### 8-Extraction:
        a. Extract parasitic capacitances, resistances, and inductances from the layout.
        b. Use this information for post-layout simulation.

 #### 9-Post-Layout Simulation:
        a. Simulate the cell with extracted parasitics to ensure it still meets performance requirements.
        b. Iterate on the design if necessary.

 #### 10-Validation and Testing:
        a. Conduct additional simulations and checks to validate the design.
        b. Perform physical testing on fabricated cells to ensure they meet specifications.

 #### 11-Library Integration:
        a. Integrate the cell into a cell library for use in larger IC designs.
        b. Ensure compatibility with other cells in the library.

#### Cell Characterization Flow

Cell characterization is the process of determining the performance characteristics of a cell under various conditions. This information is crucial for ensuring that the cell behaves as expected when used in larger circuits. The characterization flow typically involves the following steps:

    I.    Pre-Characterization Setup
    II.   Simulation
    III.  Data Collection
    IV.   Analysis
    V.    Model Generation 
    VI.   Validation
    VII.  Documentation
    VIII. Release

  ![maxresdefault](https://github.com/user-attachments/assets/795cffda-9895-48b9-83c2-44433763ab91)

  ### Timing threshold and Propagation delay:-

  #### Timing threshold:
  Timing thresholds are critical parameters in the design and analysis of digital circuits, particularly in the context of integrated circuits (ICs). They define the boundaries within which a circuit must operate to ensure reliable and correct functionality. Timing thresholds are used to determine whether a circuit meets its performance specifications, such as speed and power consumption.

  #### Propagation delay:
  Propagation delay is a critical parameter in the design and analysis of digital circuits, particularly in the context of integrated circuits (ICs). It refers to the time it takes for a signal to propagate through a circuit from the input to the output. Understanding propagation delay is essential for ensuring that a circuit meets its performance specifications, such as speed and reliability.

##### Components of Propagation Delay:

    a. Gate Delay: The time it takes for a signal to pass through a single logic gate.
    b. Interconnect Delay: The time it takes for a signal to travel along the wires or interconnects between gates.
    c. Transmission Line Effects: In high-speed circuits, the effects of transmission lines, such as reflections and signal degradation, can also contribute to propagation delay.

     
![124042219-5b203c00-d9d6-11eb-9915-79e2bcb4a506](https://github.com/user-attachments/assets/7c793d77-60de-4b05-9980-331c10c577f6)



       .










