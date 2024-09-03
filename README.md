## Digital VLSI SoC Design and Planning

## Contents
[ASIC Design Flow](#asic-design-flow)

[DAY-1: Overview of open source EDA OpenLANE and Sky130PDK](#day-1-overview-of-open-source-eda-openlane-and-sky130pdk)

[DAY-2: Good Floorplan vs bad Floorplan and Introduction to library cells](#day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)

[DAY-3: Design Library Cell using Magic Layout and NGSPICE Characterization](#day-3-design-library-cell-using-magic-layout-and-ngspice-characterization)

[DAY-4: Pre Lay-out Timing Analysis and Importance of Good clock Tree](#day-4-pre-lay-out-timing-analysis-and-importance-of-good-clock-tree)

[DAY-5: Final Steps for RTL2GDS Using TritonROUTE and openSTA](#day-5-final-steps-for-rtl2gds-using-tritonroute-and-opensta)



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








## DAY-3: Design Library Cell using Magic Layout and NGSPICE Characterization

### CONTENTS

- [VTC Spice Simulation](#vtc-spice-simulation)
- [Concept on Switching Threshold](#concept-on-switching-threshold)
- [Static And Dynamic Simulation of CMOS Inverter](#static-and-dynamic-simulation-of-cmos-inverter)
- [Layout and CMOS Fabrication Process](#layout-and-cmos-fabrication-process)
- [LABs Exercise](#labs-exercise)


### VTC Spice Simulation
VTC stands for Voltage Transfer Characteristic, which is a fundamental concept in the analysis and design of electronic circuits, particularly in the context of analog and mixed-signal integrated circuits. A VTC spice simulation refers to the process of using a SPICE (Simulation Program with Integrated Circuit Emphasis) simulator to analyze and plot the voltage transfer characteristic of a circuit.

In a VTC simulation, the input voltage to a circuit is swept across a range of values, and the corresponding output voltage is recorded. The resulting plot of output voltage versus input voltage provides insight into the linearity, gain, and operating regions of the circuit. This is particularly important for understanding the behavior of amplifiers, logic gates, and other signal processing circuits.

Here's a step-by-step overview of how a VTC spice simulation might be performed:

  #### I> Circuit Design: The circuit to be analyzed is designed in a schematic capture tool that is compatible with the SPICE simulator being used.

  #### II> Simulation Setup: The SPICE simulation is set up with the appropriate analysis type, which could be a DC sweep analysis if the goal is to plot the VTC over a range of DC input voltages.

 #### III> nput Signal Sweep: The input voltage source is configured to sweep across the desired range of voltages. This could be from the negative supply rail to the positive supply rail, or any other relevant range.

  #### IV> Output Measurement: 
   The output node of the circuit is specified, and the simulator is instructed to record the voltage at this node for each input voltage step.

 #### V> Simulation Run: The simulation is run, and the SPICE engine calculates the circuit's response to each input voltage.

  #### VI> Data Analysis: The resulting data is plotted as a graph with input voltage on the x-axis and output voltage on the y-axis. This plot is the VTC of the circuit.

#### VII>  Interpretation: The VTC is analyzed to determine the circuit's performance characteristics, such as gain, input offset voltage, output swing, and the presence of any non-linearities or distortion.

![Screenshot 2024-08-31 21 19 24](https://github.com/user-attachments/assets/8687e29c-48c4-4f27-adda-fcf81aa23011)



### Concept on Switching Threshold

The concept of the switching threshold is crucial in the context of digital circuits, particularly in logic gates and transistors. The switching threshold refers to the input voltage level at which the circuit transitions from one state to another, typically from a low (logic 0) to a high (logic 1) state or vice versa.

In a digital circuit, the switching threshold is designed to be at a specific voltage level to ensure reliable operation and to minimize the effects of noise and signal degradation. The exact value of the switching threshold can vary depending on the technology and design of the circuit, but it is typically set near the midpoint of the supply voltage range.

For example, in a CMOS (Complementary Metal-Oxide-Semiconductor) inverter, which is a fundamental building block of digital logic, the switching threshold is ideally at the midpoint of the supply voltage (Vdd/2). This ensures that the inverter has equal noise margins for both high and low input levels.

The switching threshold is influenced by several factors, including:

   1. Device Characteristics: The physical properties of the transistors used in the circuit, such as their threshold voltage (Vth), affect the overall switching threshold of the circuit.

   2. Supply Voltage: The operating voltage of the circuit can impact the switching threshold. As the supply voltage changes, the switching threshold may also shift.

   3. Temperature: Temperature variations can cause changes in the electrical characteristics of the transistors, which can in turn affect the switching threshold.

   4. Process Variations: Manufacturing variations can lead to differences in the electrical properties of transistors, which can influence the switching threshold of the circuit.

   5. Load Capacitance: The capacitive load connected to the output of the circuit can affect the switching threshold due to the dynamic behavior of the circuit during switching.

![2BqKy](https://github.com/user-attachments/assets/98bd56a3-610a-48cb-a8bc-629143427dbd)




### Static And Dynamic Simulation of CMOS Inverter

#### Static Simulation
Static simulation, also known as DC analysis, involves analyzing the circuit's behavior at DC (Direct Current) steady-state conditions. This type of simulation is used to determine the following characteristics:

   Voltage Transfer Characteristic (VTC): The VTC plot shows the relationship between the input voltage (Vin) and the output voltage (Vout) of the inverter. It helps in understanding the transition between the logic levels (0 and 1) and the noise margins.

   a- Switching Threshold (V_th): The input voltage at which the inverter switches from one logic state to another. Ideally, for a symmetrical VTC, the switching threshold is at Vdd/2, where Vdd is the supply voltage.

   b- Noise Margins: The maximum noise voltage that can be tolerated at the input while the output remains in the correct logic state.

   c- Static Power Dissipation: The power consumed by the inverter when it is in a stable state (not switching). This is typically very low in CMOS circuits due to the low leakage currents.

#### Dynamic Simulation
Dynamic simulation, also known as transient analysis, involves analyzing the circuit's behavior over time as it responds to time-varying input signals. This type of simulation is used to determine the following characteristics:

   a- Propagation Delay (t_p): The time it takes for the output to change in response to a change in the input. It is typically measured as the time from the 50% point of the input transition to the 50% point of the output transition.

   b- Rise Time (t_r) and Fall Time (t_f): The times it takes for the output to transition from 10% to 90% (rise time) and from 90% to 10% (fall time) of the output voltage swing.

   c- Power Consumption: The dynamic power consumed by the inverter during switching, which is a function of the switching frequency, load capacitance, and supply voltage.

   d- Transient Response: The overall response of the inverter to input signals, including overshoot, undershoot, and ringing.
    
![maxresdefaul4t](https://github.com/user-attachments/assets/5a72fdab-6149-4377-a1b9-b53323efa131)



### Layout and CMOS Fabrication Process

The CMOS (Complementary Metal-Oxide-Semiconductor) fabrication process is a complex series of steps used to manufacture integrated circuits. CMOS technology is widely used in modern electronics due to its low power consumption and high-density integration capabilities. The fabrication process involves several key steps, including substrate preparation, doping, oxidation, photolithography, etching, and metallization. Here is an overview of the CMOS fabrication process:
1. Substrate Preparation

    Starting Material: The process begins with a pure silicon wafer, which serves as the substrate.
    Cleaning: The wafer is thoroughly cleaned to remove any impurities.

2. Oxidation

    Thermal Oxidation: A layer of silicon dioxide (SiO2) is grown on the surface of the silicon wafer. This oxide layer acts as an insulator and also serves as a mask for subsequent doping processes.

3. Photolithography

    Photoresist Application: A light-sensitive polymer called photoresist is applied to the wafer surface.
    Exposure and Development: The wafer is exposed to ultraviolet (UV) light through a mask that contains the desired pattern. The exposed photoresist is then developed, leaving a patterned layer on the wafer.

4. Etching

    Wet or Dry Etching: The areas of the wafer not protected by the photoresist are etched away using either wet chemicals or dry plasma etching.

5. Doping

    Ion Implantation: Selected areas of the wafer are doped with impurities (dopants) such as boron or phosphorus to create n-type or p-type regions. This process alters the electrical properties of the silicon, creating the necessary semiconductor characteristics.

6. Isolation

    Local Oxidation of Silicon (LOCOS): An oxide layer is grown in selected areas to isolate different components of the circuit.
    Trench Isolation: Alternatively, trenches can be etched and filled with oxide to achieve isolation.

7. Transistor Formation

    Gate Oxide Growth: A thin layer of oxide is grown on the surface where the transistors will be formed.

    Polysilicon Deposition: A layer of polysilicon is deposited and patterned to form the gates of the transistors.

    Spacer Formation: Spacers are created on the sides of the gate to control the subsequent doping process.

    Source/Drain Implantation: Dopants are implanted to form the source and drain regions of the transistors.

8. Silicidation

    Silicide Formation: A silicide layer (e.g., tungsten silicide) is formed on top of the polysilicon gate and the source/drain regions to reduce resistance.

9. Interlayer Dielectric and Planarization

    Dielectric Deposition: A layer of insulating material (e.g., silicon dioxide) is deposited over the entire wafer.
    Chemical Mechanical Polishing (CMP): The wafer surface is planarized using CMP to ensure a smooth surface for subsequent metallization.

10. Metallization

    Metal Layers: Multiple layers of metal (e.g., aluminum or copper) are deposited and patterned to form the interconnections between transistors and other components.
    Contact and Via Formation: Contacts and vias are etched and filled with metal to connect the transistors to the metal layers.

11. Passivation

    Passivation Layer: A final layer of insulating material is deposited to protect the circuit from external contaminants.

12. Testing and Packaging

    Wafer Probing: The wafer is tested to identify any defective chips.

    Dicing: The wafer is cut into individual chips (dice).

    Packaging: The chips are mounted in packages, which are then sealed to protect the delicate circuitry.

    Final Testing: The packaged chips are tested again to ensure they meet the required specifications.

![Assigning-the-names-of-NMOS-and-PMOS](https://github.com/user-attachments/assets/936498d3-3e2e-41d5-8ee4-da89fd7c5e7c)

![Screenshot 2024-08-31 16 28 33](https://github.com/user-attachments/assets/d2f5f5f2-9e34-4a67-8bc8-ec337f192e6c)


### LABs Exercise

#### 1. IoPlacer revision & adding the "set::env" command

![copy_of_sky130](https://github.com/user-attachments/assets/7a93c68a-0453-4cec-8a6a-ce2475ec8dcf)

![Screenshot from 2024-08-29 17-35-05](https://github.com/user-attachments/assets/b3901dc3-ed2d-48a2-a770-df2566893c80)

here the the i/o pins are congested and overlapped:
![Screenshot from 2024-08-29 17-40-03](https://github.com/user-attachments/assets/b753f254-58f2-4765-85a2-62cb961207a2)

#### 2. Inverter layout in Magic tool

![Screenshot from 2024-08-29 18-47-01](https://github.com/user-attachments/assets/8383d237-ac12-4b89-bafb-98c06054e61f)

![Screenshot from 2024-08-29 18-54-02](https://github.com/user-attachments/assets/ea664cd7-cb07-442e-838b-e7f4ab867fb1)

#### 3. Extraction to spice format

![Screenshot from 2024-08-29 19-08-56](https://github.com/user-attachments/assets/d323e7d9-33d4-4830-b8ca-8c272448b887)

![Screenshot from 2024-08-29 19-10-00](https://github.com/user-attachments/assets/c66868ba-5ade-4a03-993d-28590ebd35fb)

![Screenshot from 2024-08-29 19-10-45](https://github.com/user-attachments/assets/f35f1939-cda1-4ecf-a890-53af26cb7bce)

#### 4. spice tecgnology file, simulation and output graph

![Screenshot from 2024-08-29 19-10-45](https://github.com/user-attachments/assets/6f227d4a-0ba5-44cf-a165-52d39c790ccf)

![Screenshot from 2024-08-30 18-31-52](https://github.com/user-attachments/assets/c8c56581-42d2-482d-8799-bbcfa6f89ad5)

![Screenshot from 2024-08-30 01-51-46](https://github.com/user-attachments/assets/6ea7716f-15a0-4842-8202-7422e3a86411)

updated tech file:
![Screenshot from 2024-08-30 18-32-22](https://github.com/user-attachments/assets/fefa7871-be72-49ae-b910-8aeb77211ac4)

#### 5. Parameters Calculation
##### 5-a rise transition of of output: 20% of VDD to 80% of VDD
rise_transition = (80% of 3.3v) - (20% of 3.3v)
                = (x-coordinate pos. of 2.64v - x-coordinate pos. of 0.66v)
                = (6.24247*e-19) - (6.61804*e-19)
                = 0.06163 ns

##### 5-b fall transition of of output: 80% of VDD to 20% of VDD
fall_transition = (20% of 3.3v) - (80% of 3.3v)
                = (x-coordinate pos. of 0.66v - x-coordinate pos. of 2.64v)
                = (8.09409*e-19) - (8.05156*e-19)
                = 0.04253 ns
                
   ![Screenshot from 2024-08-30 18-18-42](https://github.com/user-attachments/assets/7fc13cca-f173-40e0-8c3b-ac0d6a6858c8)

                
 ##### 5-c cell rise delay: (50% of o/p rise) - (50% of i/p fall)
 cell_rise_delay= (x-coordinate pos. of 1.65v - x-coordinate pos. of 1.65v)
                = (6.2089*e-19) - (6.15*e-19)
                = 0.0589 ns
                
   ![Screenshot from 2024-08-30 18-24-04](https://github.com/user-attachments/assets/8f885539-8b54-4e08-b39b-6eb989749332)

 ##### 5-d cell fall delay: (50% of o/p fall) - (50% of i/p rise)
 cell_fall_delay= (x-coordinate pos. of 1.65v - x-coordinate pos. of 1.65v)
                = (8.07657*e-19) - (8.05*e-19)
                = 0.02657 ns
                
   ![Screenshot from 2024-08-30 18-26-33](https://github.com/user-attachments/assets/9636b765-cf0f-4b80-9bd0-c28927982734)


   ##### 6. Introduction to Magic tool and DRC

![Screenshot from 2024-08-30 23-22-53](https://github.com/user-attachments/assets/23ff1ac9-6b2f-440e-843b-77a82dc0635f)

![Screenshot from 2024-08-30 23-37-07](https://github.com/user-attachments/assets/0c2c91cc-a865-477c-abdc-9c469bd7f1c7)

![Screenshot from 2024-08-30 23-38-48](https://github.com/user-attachments/assets/6c03bc22-8909-492f-ad8d-6a04e9bc2516)

![Screenshot from 2024-08-31 00-13-06](https://github.com/user-attachments/assets/e9368ab6-47cb-4a22-b284-549108f167b9)

![Screenshot from 2024-08-31 00-22-14](https://github.com/user-attachments/assets/d3aa46d6-bbde-4273-b744-910a597e6f75)

![Screenshot from 2024-08-31 00-27-51](https://github.com/user-attachments/assets/3eb2dc89-30be-4c20-a408-adec09a08a4b)

![Screenshot from 2024-08-31 00-41-17](https://github.com/user-attachments/assets/6aa6b086-f3a1-4599-8aa3-f7cc21689653)





## DAY-4: Pre Lay-out Timing Analysis and Importance of Good clock Tree

### CONTENTS

- [1-Delay Table](#1-delay-table)
- [2-Setup time and Hold time of Flop](#2-setup-time-and-hold-time-of-flop)
- [3-Clock tree routing and buffering](#3-clock-tree-routing-and-buffering)
- [4-LABs Steps](#4-labs-steps)




### 1-Delay Table

In physical design, a delay table is a data structure used to model the delay characteristics of standard cells or interconnects in a digital circuit.

#### Purpose
Estimate Delays: Delay tables help in estimating the propagation delay through gates or along wires, which is crucial for timing analysis.

#### Components
Input Slew: The rise and fall time of the input signal.

Output Load: The capacitive load that the gate drives, including wire capacitance and the input capacitance of connected gates.

#### Delay Values: The actual propagation delays, typically provided for different combinations of input slew and output load.

#### Usage
Static Timing Analysis (STA): Delay tables feed information into STA tools to evaluate the timing performance of a design.

Cell Libraries: Standard cell libraries include delay tables for each cell type, allowing EDA tools to accurately model and simulate circuit behavior.
#### Types
Linear Delay Models: Simplified models that use linear equations.

Non-linear Delay Models: More complex and accurate, capturing variations due to non-linear effects.
#### Importance
Accuracy: Provides accurate timing information, crucial for ensuring that the design meets its timing constraints.

Optimization: Helps in identifying critical paths and optimizing them to improve performance.

![Screenshot from 2024-09-03 19-46-27](https://github.com/user-attachments/assets/3accdf7b-aa36-4e30-91b4-a7f8ad3b2006)



### 2-Setup time and Hold time of Flop

#### Set Up Time Analysis
Setup time is the minimum time period before the clock edge during which the data input must be stable.

#### Purpose:
Ensures that the data is correctly sampled by the flip-flop on the active clock edge.

#### Analysis Steps:
Identify Critical Paths: Trace the longest path from one flip-flop to the next, including combinational logic.

#### Calculate Data Path Delay: Sum up the delays of all elements (gates, interconnects) in the data path.

#### Compare with Setup Time: Ensure that the data path delay plus setup time is less than the clock period.

Data Path Delay + Setup Time < Clock Period

Adjust if Necessary: If the condition isn’t met, optimize the design by reducing delays, increasing clock period, or modifying the path.

#### Hold Time Analysis
Hold time is the minimum time period after the clock edge during which the data input must remain stable.

#### Purpose:
Prevents the new data from being captured too early, ensuring the current data is held long enough.

#### Analysis Steps:
Identify Critical Paths: Examine paths where new data might overwrite current data too soon.

Calculate Data Path Delay: Consider the minimum delay from the clock edge to the data input.

Compare with Hold Time: Ensure that the data path delay is greater than the hold time.

Data Path Delay>Hold Time

Adjust if Necessary: If the condition isn’t met, add buffers or delay elements to increase path delay.

#### Importance
Setup Time Violations: Can lead to incorrect data being captured, causing functional errors.

Hold Time Violations: Can result in data corruption as new data overwrites old data prematurely.

![Screenshot from 2024-09-03 21-43-49](https://github.com/user-attachments/assets/f0228524-7072-4822-87b6-0aaff1b40980)




### 3-Clock tree routing and buffering

Clock tree routing and buffering are crucial steps in the physical design phase of integrated circuit (IC) design. These steps ensure that the clock signal is distributed efficiently and uniformly across the entire chip to all sequential elements (like flip-flops) with minimal skew and latency.

#### a. Clock Tree Synthesis (CTS):
#### Purpose: The goal of clock tree synthesis is to distribute the clock signal from a single clock source (usually a Phase-Locked Loop (PLL) or a clock generator) to all the sequential elements in the design (like flip-flops) with minimal skew and balanced delays.

#### Challenges:
Clock Skew: The difference in arrival times of the clock signal at different sequential elements. Skew can lead to timing violations, so minimizing it is a primary goal.

Clock Latency: The delay from the clock source to a flip-flop or other sequential element. Latency needs to be controlled to meet timing requirements.

#### Power Consumption: Clock networks are often the most power-consuming part of the chip. Optimizing for lower power while maintaining performance is crucial.

#### b. Clock Tree Routing:

#### Tree Structure: The clock distribution network is usually constructed as a tree (hence the name "clock tree"). This tree structure helps in balancing the delays and skew.

#### Clock Tree Topologies:
H-Tree: A hierarchical tree structure that is symmetric and balanced, often used in regular grid layouts.

X-Tree, Y-Tree: Variations of the H-tree, used based on specific design needs.

Spine: A clock distribution method where the clock is routed along a central "spine" and branches out to different regions. This is common in large, hierarchical designs.

Routing Techniques:
Minimal Skew Routing: Ensuring that all paths from the clock source to the clock sinks (sequential elements) are balanced to minimize skew.
Buffered Clock Tree: Inserting buffers along the clock tree to manage delay and drive strength, ensuring that the clock signal reaches all parts of the circuit with sufficient strength and minimal degradation.

#### c. Clock Tree Buffering:
#### Why Buffers are Needed:
Load Management: The clock signal needs to drive a large number of flip-flops and other clocked elements. Buffers are used to amplify the clock signal and drive these loads effectively.
Delay Control: Buffers help in balancing the delay in different paths of the clock tree, which is crucial for minimizing skew.
Noise Reduction: By buffering the clock signal, noise introduced by long interconnects can be reduced.

Types of Buffers:
Inverters: Simple buffers that invert the signal. Sometimes used in pairs to maintain the same logic level while buffering.
Dedicated Clock Buffers: Specially designed buffers that are optimized for driving the clock signal with high fan-out and minimal jitter.
Buffer Insertion:
Buffers are strategically inserted at points in the clock tree where the clock signal needs to be amplified or where delay needs to be controlled.
The placement of these buffers is determined during the Clock Tree Synthesis process using EDA tools, which optimize for delay, skew, and power consumption.

#### d. Skew and Latency Management:
Zero-Skew Clock Tree: An ideal clock tree would have zero skew, meaning all flip-flops receive the clock signal at the exact same time. While practically impossible, the goal is to minimize skew as much as possible.
Useful Skew: Sometimes, intentional skew is introduced to improve timing in certain paths. This is known as useful skew and can help in meeting setup and hold time constraints.
Clock Latency: Clock tree buffering and routing also ensure that the clock signal arrives at the flip-flops with the desired latency, which is crucial for timing closure.

#### e. Post-CTS Optimization:
After the clock tree is synthesized, further optimization steps like Clock Tree Optimization (CTO) and Post-CTS Optimization might be performed to fine-tune the clock network, ensuring that all timing requirements are met.
Timing Analysis: Tools perform static timing analysis (STA) to verify that the clock tree meets the required timing constraints, and any violations are corrected by adjusting the clock tree design.

![Screenshot from 2024-09-03 22-05-22](https://github.com/user-attachments/assets/3592f0f9-aa1b-4356-878a-5bb2c7cdaa2f)

![Screenshot from 2024-09-03 22-05-45](https://github.com/user-attachments/assets/685395e4-8914-46e9-b331-f2ab999e09d5)


### 4-LABs Steps:

tracks.info 
![Screenshot from 2024-09-01 15-25-09](https://github.com/user-attachments/assets/3a412cc9-1e37-40b5-bec7-4e8fed3a55b3)
![Screenshot from 2024-09-01 15-25-16](https://github.com/user-attachments/assets/4a93476f-a381-4a95-a628-832e278704de)

Inverter_mag
![Screenshot from 2024-09-01 15-41-31](https://github.com/user-attachments/assets/9b3f5218-fdfa-4fb1-b810-a084c765da39)
![Screenshot from 2024-09-01 15-51-50](https://github.com/user-attachments/assets/d5e6230f-b442-4d91-9899-878e348cc3b0)
![Screenshot from 2024-09-01 15-57-15](https://github.com/user-attachments/assets/f49a4b1e-9590-4fbb-84c2-9763d7a4256f)
![Screenshot from 2024-09-01 15-59-45](https://github.com/user-attachments/assets/8fe708d2-f9f6-4a1b-9cdf-a28bec8df153)

copying the inverter lef to my design/src
![Screenshot from 2024-09-01 16-16-05](https://github.com/user-attachments/assets/e45d8af5-935d-4ea8-9bfb-341bc5377611)

lib file
![Screenshot from 2024-09-01 16-18-24](https://github.com/user-attachments/assets/cb294fc0-451a-4f43-9cc9-a137a1a7f700)

config.tcl
![Screenshot from 2024-09-01 17-20-31](https://github.com/user-attachments/assets/afcb408e-0f43-4c86-af0c-37de6f6cafa3)
![Screenshot from 2024-09-01 22-11-26](https://github.com/user-attachments/assets/40ed353e-83e3-4df3-84f2-552575f021fc)

run_synthesis
![Screenshot from 2024-09-01 22-11-31](https://github.com/user-attachments/assets/e94502a2-7a97-48db-becd-fa80f0b628e5)
![Screenshot from 2024-09-01 22-39-37](https://github.com/user-attachments/assets/38b4593b-fedd-4933-971a-d8d8adc6a5fe)
![Screenshot from 2024-09-02 12-59-04](https://github.com/user-attachments/assets/26524dc2-860f-4336-840e-1b7c8ffb5a62)

run_floorplan
![Screenshot from 2024-09-02 13-06-45](https://github.com/user-attachments/assets/4e7932f2-1f6d-4fe1-a8c4-fe3f00e71ceb)

run init_floorplan
run place_io
tap_decap_or
![Screenshot from 2024-09-02 13-06-45](https://github.com/user-attachments/assets/7349c5b1-2318-4be3-b6bf-61d4cb4ba5ab)
![Screenshot from 2024-09-02 13-10-47](https://github.com/user-attachments/assets/ae73e2b6-ee21-4935-b970-8e6d12e35677)

run_placement
![Screenshot from 2024-09-02 21-46-22](https://github.com/user-attachments/assets/99f39301-b6c0-42dd-b23d-578481a08dc2)

lay-out
![Screenshot from 2024-09-02 21-54-33](https://github.com/user-attachments/assets/fc89caf7-c45f-484e-8e87-0a0fbf510894)
![7cdb4291-03a6-44e5-8e6a-a2f0253515f5](https://github.com/user-attachments/assets/c727abeb-2082-4da4-9b38-0cf48d121e74)

my_base.sdc
![Screenshot from 2024-09-02 23-52-01](https://github.com/user-attachments/assets/78fedd02-a835-4856-bd02-83fceb7bfe2b)

sta.conf
![Screenshot from 2024-09-02 23-52-20](https://github.com/user-attachments/assets/c5531bfa-1e00-4438-b2b6-9e2c90ea4210)
![Screenshot from 2024-09-03 00-01-51](https://github.com/user-attachments/assets/43d4cd03-c20a-4e49-8562-b9e73c88674f)

again run_synthesis, then floorplan, placement
![Screenshot from 2024-09-03 00-07-12](https://github.com/user-attachments/assets/445a92eb-20dd-4199-923b-562bd6dace9d)
![Screenshot from 2024-09-03 00-07-25](https://github.com/user-attachments/assets/24fab7a5-3b58-4d4a-9861-24a821ad854d)
![Screenshot from 2024-09-03 11-41-37](https://github.com/user-attachments/assets/5ce247e1-0dcb-4227-b271-db97f740a71c)

slacl reducing
![Screenshot from 2024-09-03 01-54-15](https://github.com/user-attachments/assets/c13e2e37-9692-472a-876b-9025fbcd8049)
![Screenshot from 2024-09-03 11-41-37](https://github.com/user-attachments/assets/c8ed7b85-aeff-41b9-bebc-d19362855119)

openroad
![Screenshot from 2024-09-03 12-01-28](https://github.com/user-attachments/assets/4d9e76e9-02b2-4723-91cf-92d7ea99386b)

CTS run
![Screenshot from 2024-09-03 15-33-21](https://github.com/user-attachments/assets/e09ddcfb-b9a3-434a-812d-ca5fa73112fb)
![Screenshot from 2024-09-03 11-43-15](https://github.com/user-attachments/assets/44374bb3-3e70-4ff5-874b-48292ec306b2)
![Screenshot from 2024-09-03 12-01-28](https://github.com/user-attachments/assets/8285663a-ce94-4f0a-b952-4de128c9c252)
![Screenshot from 2024-09-03 15-33-21](https://github.com/user-attachments/assets/4e3815c9-d0f7-4294-957e-73ac623f152d)
![Screenshot from 2024-09-03 15-35-15](https://github.com/user-attachments/assets/b96fa9ef-4dc0-4fe3-ba8f-81833b9023a1)





## DAY-5: Final Steps for RTL2GDS Using TritonROUTE and openSTA

### CONTENTS

- [1-Routing and DRC](#1-routing-and-drc)
- [2-Global routing and Detailed routing](#2-global-routing-and-detailed-routing)
- [3-Maze routing and Steiner tree algorithm](#3-maze-routing-and-steiner-tree-algorithm)
- [4-Power distribution and network routing](#4-power-distribution-and-network-routing)
- [5-TritonRoute features](#5-tritonroute-features)
- [6-Labs practise](#6-labs-practise)



### 1-Routing and DRC
Routing and Design Rule Check (DRC) are key steps in the physical design process of integrated circuits (ICs). They play crucial roles in ensuring that the design is manufacturable and meets all the required electrical and physical constraints.

#### a. Routing:
Routing is the process of connecting the various components (standard cells, macros, IOs, etc.) in an IC design with metal interconnects according to the netlist generated during the synthesis stage.

#### Key Aspects of Routing:
Global Routing:

Purpose: Provides an abstract, coarse-grained plan of how the connections will be made across different regions of the chip. It divides the chip area into a grid and assigns paths to nets without detailed geometry.
Outcome: Guides the detailed routing stage by giving an overall connectivity layout.
Detailed Routing:

Purpose: Converts the global routing plan into actual geometrical paths on the metal layers of the chip.
Metal Layers: The routing is done using different metal layers, with lower layers typically used for local connections and higher layers for long-distance or global connections.

#### Routing Algorithms:
Maze Routing: Finds the shortest path between two points, avoiding obstacles.
Channel Routing: Manages routing in channels between rows of cells.
Grid-Based Routing: Uses a grid to guide the routing paths and ensure that they follow design rules.
Routing Challenges:

Congestion: Too many wires trying to pass through the same area can lead to congestion, which can cause timing issues or even make routing impossible.
Crosstalk: Signals on adjacent wires can interfere with each other, leading to noise and potential errors.
Delay: Longer or more resistive paths can introduce delays that affect the timing of the circuit.
Routing Constraints:

#### Timing Constraints: Ensuring that the routed paths meet the required timing (setup and hold times).
Power Constraints: Minimizing power consumption and ensuring that power distribution is balanced.
Signal Integrity: Maintaining signal quality by minimizing noise, crosstalk, and other electrical issues.

#### b. Design Rule Check (DRC):
Design Rule Check (DRC) is a verification step in the physical design process that ensures the layout of the chip adheres to a set of predefined rules provided by the semiconductor foundry. These rules are necessary to guarantee that the design can be manufactured reliably.

#### Key Aspects of DRC:
Design Rules:

Spacing Rules: Minimum distances between different metal layers, vias, or other features to avoid shorts and ensure manufacturability.
Width Rules: Minimum and maximum width of wires to ensure that they can be manufactured correctly and carry the required current without issues.
Enclosure Rules: Requirements for how different layers must overlap or enclose each other (e.g., metal layers and vias).
Alignment Rules: Ensure that different layers align correctly to avoid misalignment during manufacturing.

DRC Tools:

Tools like Calibre, Mentor Graphics, or Cadence's Assura are commonly used to run DRC checks. These tools take the physical layout as input and verify it against the foundry's design rules.
Common DRC Violations:

Shorts: When two wires or components that should not be connected are too close or overlap.
Opens: Missing connections due to routing errors or insufficient overlap of layers.
Minimum Width Violations: Wires or features that are too narrow and might not be reliably manufactured.
Spacing Violations: Features that are too close together, which can cause shorts or manufacturing issues.
DRC Correction:

After running a DRC, any violations are flagged, and the design must be corrected. This may involve adjusting the layout, rerouting wires, or modifying cell placement.

![Screenshot from 2024-09-04 00-25-49](https://github.com/user-attachments/assets/de745192-63fb-4d8e-81cc-4091f6333ff6)
![Screenshot from 2024-09-04 00-26-08](https://github.com/user-attachments/assets/9b3dd792-b4b0-4183-943d-ea7bd653bcb2)



### 2-Global routing and Detailed routing

#### a. Global Routing:

##### Purpose: Provides a high-level, coarse plan for connecting different regions of the chip. It divides the chip into grids and assigns general paths for connections without specifying exact wire routes.
##### Outcome: Guides the detailed routing stage by outlining broad paths for signals to follow, helping to manage congestion and ensure that connections can be made.

#### b. Detailed Routing:

##### Purpose: Converts the global routing plan into precise, geometric wire routes on specific metal layers, connecting all components according to the design's netlist.
##### Outcome: Finalizes the exact paths for each wire, ensuring they meet design rules (like spacing and width), and resolves any conflicts or congestion identified during global routing.

![Screenshot from 2024-09-04 01-01-55](https://github.com/user-attachments/assets/60b5b7d6-ed70-4ab7-8ffc-c50d98ec4ba1)


### 3-Maze routing and Steiner tree algorithm

#### a. Maze Routing:

Purpose: Maze routing is an algorithm used to find a path between two points (e.g., from a source to a destination pin) in a grid while avoiding obstacles.

How It Works:

The grid is treated as a matrix, where each cell represents a possible location for the wire. Obstacles like other wires, cells, or blocked areas are marked as unavailable.
The algorithm explores all possible paths from the source to the destination, usually using a breadth-first search (BFS) approach.
It expands from the source node by checking neighboring cells until it reaches the destination.
The shortest path found through this exploration is chosen as the route.
Pros:

Optimal Path: Guarantees finding the shortest path if one exists.
Flexibility: Can handle complex obstacles and multiple routing layers.
Cons:

Computationally Expensive: Can be slow and resource-intensive, especially for large designs.
Not Always Practical for Large Grids: The algorithm can become infeasible for very large or densely populated grids due to its exhaustive nature.

#### b. Steiner Tree Algorithm:
Purpose: The Steiner tree algorithm is used to connect multiple points (e.g., multiple pins that need to be connected by a single net) with the shortest possible interconnect length, minimizing the total wire length.

How It Works:

The algorithm starts by creating a minimal spanning tree (MST) that connects all the target points (e.g., pins).
Steiner points, which are additional points not originally in the set of target points, are introduced to reduce the overall wire length. These points act as intermediate nodes that help in minimizing the total connection length.
The resulting structure is a Steiner tree, which is a tree that connects all target points (and possibly additional Steiner points) with the minimal total interconnect length.
Pros:

Minimized Wire Length: Produces a routing solution with minimal total wire length, which is beneficial for reducing delay and power consumption.
Efficient for Multi-Terminal Nets: Particularly useful for nets that need to connect more than two pins.
Cons:

Complexity: Finding the exact Steiner tree is an NP-hard problem, meaning it can be computationally expensive for large nets.
Approximation: Often, heuristic or approximate methods are used to find a near-optimal Steiner tree, which might not be the absolute minimum.

![Screenshot from 2024-09-04 00-45-15](https://github.com/user-attachments/assets/f10eb3e1-abcd-49d3-8ef7-f201b5c945ba)
![Screenshot from 2024-09-04 00-45-34](https://github.com/user-attachments/assets/e14c7003-6fda-431b-8e08-100263c39ef2)
![Screenshot from 2024-09-04 00-45-49](https://github.com/user-attachments/assets/e8755cd7-549d-4899-9d29-79c55d5f5031)



### 4-Power distribution and network routing

#### a. Power Distribution:
Power distribution in IC design refers to the process of delivering power from the external power sources (like power pads or pins) to every component within the chip, including logic gates, flip-flops, memory cells, and other circuits.

##### Key Concepts:

##### Power and Ground Rails:
These are the main conductors that distribute power (VDD) and ground (VSS) across the chip. They are typically implemented as wide metal lines running across the chip to minimize resistance and voltage drop.

##### Power Grids:
A power grid is a network of horizontal and vertical metal lines that form a mesh-like structure across the chip. This grid ensures that power is distributed uniformly to all areas of the chip.
VDD Grid: Carries the supply voltage.
VSS Grid: Carries the ground potential.

##### Power Rings:
Rings of metal lines surrounding certain regions or blocks on the chip, providing a stable supply of power and grounding. They help in minimizing noise and ensuring a reliable power supply.

##### Power Straps:
Wider metal lines or groups of parallel lines that provide robust connections between the power grid and the power sources. These straps help reduce resistance and voltage drop.

##### Decoupling Capacitors:
Capacitors placed close to the power pins of circuits to stabilize the power supply by filtering out noise and compensating for sudden changes in current demand.

#### b. Power Network Routing:
Power network routing involves the detailed placement and routing of the power distribution network (PDN) on the chip, ensuring that all blocks and standard cells receive adequate power.

##### Steps in Power Network Routing:

##### Placement of Power Pads:
Power pads (VDD, VSS) are placed around the periphery of the chip or in specific locations within the chip. These pads connect the internal power network to the external power supply.

##### Creation of Power Grid:
A grid of metal layers is laid out across the chip to distribute the power. The grid usually spans multiple metal layers, with the lower layers used for local distribution and the upper layers for global distribution.

##### Power Rings and Straps:
Power rings are placed around the periphery of major blocks (e.g., memory blocks, large logic blocks) to ensure a stable power supply.
Power straps are used to connect the power rings and the power grid, ensuring that current can flow efficiently from the power pads to all parts of the chip.

##### Via Placement:
Vias are used to connect different metal layers within the power grid. Multiple vias are often placed in parallel to reduce resistance and ensure reliable current flow.

##### IR Drop Analysis:
After routing the power network, an IR drop analysis is performed to ensure that the voltage drop across the power grid is within acceptable limits. Excessive IR drop can cause circuits to malfunction due to insufficient voltage levels.

##### Electromigration Check:
Electromigration refers to the gradual movement of metal atoms in the power grid due to high current densities, which can lead to failures. The power network is checked to ensure that the current density in all metal lines is below the safe limits to avoid electromigration.

##### Noise and Decoupling:
The power network is designed to minimize noise (e.g., ground bounce) by carefully placing decoupling capacitors and ensuring that the grid structure is robust.

![Screenshot from 2024-09-04 00-59-03](https://github.com/user-attachments/assets/cdbff524-e23c-4c85-9162-34b2e208ce8d)




### 5-TritonRoute features
TritonRoute is an open-source detailed routing tool used in the physical design of integrated circuits (ICs). It is a critical component of the Triton suite of EDA (Electronic Design Automation) tools. TritonRoute is specifically designed for the detailed routing stage of VLSI (Very Large Scale Integration) design.

Key Features of TritonRoute:
Detailed Routing:

TritonRoute performs the detailed routing step in the IC design flow, where it converts global routing paths into exact geometrical wire routes on the metal layers of the chip.
It handles the connection of signal nets, power nets, and clock nets, ensuring that all routing adheres to design rules.
Design Rule Checking (DRC):

TritonRoute incorporates design rule checking within its routing algorithms to ensure that all routed wires comply with the foundry's design rules, such as spacing, width, and via requirements.
The tool is capable of resolving DRC violations that may arise during routing.
Open-Source and Integration:

TritonRoute is open-source, which means it can be freely used, modified, and integrated into custom EDA flows.
It is part of the broader OpenROAD project, which aims to provide a fully autonomous, open-source RTL-to-GDSII flow.
High-Quality Routing:

The tool focuses on generating high-quality routing solutions that minimize wire length, reduce congestion, and respect timing and power constraints.
It can handle complex designs with a large number of nets and multiple metal layers.
Scalability:

TritonRoute is designed to be scalable, handling both small and large designs efficiently. It leverages modern algorithms to manage the complexity of routing in advanced technology nodes.
Customization and Flexibility:

Being open-source, TritonRoute allows for extensive customization. Users can modify the tool to suit specific design requirements or integrate it with other tools in their design flow.
The tool can be adapted for different technology nodes and design styles.
Part of the Triton Suite:

TritonRoute works in conjunction with other tools in the Triton suite, such as TritonCTS (Clock Tree Synthesis) and TritonPlace (Placement), providing a comprehensive solution for physical design.
Community and Support:

As an open-source project, TritonRoute benefits from community contributions and ongoing development. It is actively maintained by researchers and contributors in the EDA community.

![Screenshot from 2024-09-04 01-07-12](https://github.com/user-attachments/assets/c6d657d9-fb20-47cd-987f-bf5396563fe1)
![Screenshot from 2024-09-04 01-07-40](https://github.com/user-attachments/assets/ae3d8a68-aa5c-49ec-bbee-c1bb00420a06)
![Screenshot from 2024-09-04 01-08-24](https://github.com/user-attachments/assets/3bb0926d-fd52-4ebe-b417-95f9ccbbc4a1)



### 6-Labs practise

![45aa96bb-7bfc-47f6-9e68-32965104e4d9](https://github.com/user-attachments/assets/1daa8a1a-3d03-4bf6-afec-ed8ed0c640de)
![97020c9a-4804-44ec-9dbc-3c888a31c74d](https://github.com/user-attachments/assets/550bd942-321a-4066-959c-abb7d13700fa)
![7f266a21-d2a7-4bee-9f74-aeec5c9ddf82](https://github.com/user-attachments/assets/aedeac6a-cfb4-4269-92a2-919e2b1f5c2f)
![bb469b51-769c-408b-8bbe-5c7e59c6870c](https://github.com/user-attachments/assets/0d838e4c-ebee-4305-ac48-6eaad362e3e9)
![ac832fc1-5798-4ab1-90d1-513754c44919](https://github.com/user-attachments/assets/b2450cc5-ae3d-4256-867a-907cd899cd75)
![34bfdaae-603e-493a-98a6-20be76519035](https://github.com/user-attachments/assets/635c710e-299c-4f85-98c0-1c65171a53ed)
![6dbcb6f0-bf28-4e85-acba-03eda3ec56ee](https://github.com/user-attachments/assets/f905c566-3969-4be5-995b-3b4b1ab961e2)
![aff86155-b67e-44d9-9ac9-583f2a2d9ec2](https://github.com/user-attachments/assets/166d4152-8c12-4cd4-86cd-fce2e22c54d3)
![7d38b0ba-3ac6-4773-bd9f-306e6e31f56f](https://github.com/user-attachments/assets/7ba075d9-9fc4-43d3-a2ad-c6407d57e7aa)









