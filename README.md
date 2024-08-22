## Digital VLSI SoC Design and Planning

## Contents
[ASIC Design Flow](#asic-design-flow)

[DAY-1: Overview of open source EDA OpenLANE and Sky130PDK](#day-1-overview-of-open-source-eda-openlane-and-sky130pdk)


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



#### some of my LINUX code snippets of 1st day task

/home/vsduser/Pictures/Screenshot from 2024-08-22 19-45-35.png
/home/vsduser/Pictures/Screenshot from 2024-08-22 22-27-12.png
/home/vsduser/Pictures/Screenshot from 2024-08-22 22-28-26.png
/home/vsduser/Pictures/Screenshot from 2024-08-22 22-49-36.png
/home/vsduser/Pictures/Screenshot from 2024-08-22 23-07-28.png
/home/vsduser/Pictures/Screenshot from 2024-08-22 23-08-43.png

