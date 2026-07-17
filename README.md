# VSD SoC Design Workshop

## Author
**Harshita Arun Salehittal**

This repository contains my notes, commands, observations, and screenshots from the **VSD SoC Design Workshop**. Throughout this workshop, I explored the complete ASIC Physical Design flow using **OpenLANE**, **Sky130 PDK**, **Magic**, **Netgen**, and other open-source EDA tools.

---

# Day 1 – Introduction to Open-Source ASIC Design and OpenLANE

## Objective

The first day focused on understanding the complete ASIC design flow, learning about open-source EDA tools, setting up the OpenLANE environment, and exploring the Sky130 Process Design Kit (PDK).

---

## ASIC Design Flow

The standard ASIC implementation process consists of the following stages:

- RTL Design
- Logic Synthesis
- Floorplanning
- Placement
- Clock Tree Synthesis (CTS)
- Routing
- Static Timing Analysis (STA)
- Physical Verification
- GDSII Generation

Each stage converts the design into a more detailed physical representation until it is ready for fabrication.

---

## OpenLANE Flow

OpenLANE is an automated RTL-to-GDSII flow built using several open-source EDA tools.

Major tools included in OpenLANE are:

- Yosys
- OpenROAD
- Magic
- Netgen
- TritonRoute
- OpenSTA
- KLayout

These tools work together to automate the ASIC implementation process.

---

## Sky130 PDK

The Sky130 Process Design Kit provides all the technology files required for physical design.

It includes:

- Standard Cell Libraries
- LEF Files
- Liberty Timing Files
- GDS Layout Files
- SPICE Models
- DRC Rules
- LVS Rules

The Sky130 PDK is developed by Google and SkyWater Technology and is widely used for open-source ASIC design.

---

## Exploring the Directory Structure

The workshop environment contains different directories that are required during implementation.

### tools/

The **tools** directory stores the software packages used throughout the workshop, including OpenLANE and supporting EDA applications.

**Screenshot**

![Tools Directory](images/day1/tools_directory.png)

---

### openlane_working_dir/

This directory contains the OpenLANE flow and the Sky130 Process Design Kit.

Important folders include:

- openlane
- pdks

**Screenshot**
<img width="1366" height="662" alt="imagesday1openlane_directory" src="https://github.com/user-attachments/assets/bea37d90-d006-4121-9548-b863767d7442" />



---

### PDK Structure

The PDK directory stores all technology-related files required for ASIC implementation.

Important contents include:

- Libraries
- Technology LEF
- Standard Cell LEF
- Timing Libraries
- SPICE Models

**Screenshot**




---

## Launching Docker

The workshop environment is started using Docker.

Example command:

```bash
docker
```

---

## Starting OpenLANE

Navigate to the OpenLANE directory.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```

Run Docker.

```bash
docker
```

Start OpenLANE.

```bash
./flow.tcl -interactive
```

Load the required package.

```tcl
package require openlane 0.9
```

---
<img width="1280" height="768" alt="Screenshot from 2026-07-13 09-46-39" src="https://github.com/user-attachments/assets/659e3668-7d99-4d83-957c-6b6d089b49e9" />

## Design Preparation


Prepare the design before running synthesis.

```tcl
prep -design picorv32a
```


This command creates the working directory and loads all configuration files needed for implementation.

**Screenshot**
<img width="1280" height="768" alt="Screenshot from 2026-07-13 09-56-06" src="https://github.com/user-attachments/assets/155c4e72-9d5b-4170-978a-93b28aea1f00" />


---

## Running Logic Synthesis

Logic synthesis converts the Verilog RTL into a gate-level netlist.

Command:

```tcl
run_synthesis
```

After synthesis, OpenLANE generates reports containing:

- Cell Count
- Area
- Timing
- Flip-Flop Statistics
- Gate Utilization

**Screenshot**

<img width="1280" height="768" alt="Screenshot from 2026-07-13 10-08-01" src="https://github.com/user-attachments/assets/b1526bc6-3e7a-472c-83f8-7697ac643c7b" />


---

## Important Results

During synthesis, the following information can be observed:

- Total Standard Cells
- Combinational Logic Cells
- Sequential Cells
- Chip Area
- Worst Slack
- Maximum Fanout

These reports help evaluate the synthesized design before moving to physical implementation.

---
<img width="1280" height="768" alt="Screenshot from 2026-07-13 13-57-42" src="https://github.com/user-attachments/assets/d996889d-6610-448a-98a6-7a646bdf6713" />
Calculation of Flop Ratio and DFF % from synthesis statistics report file
Flop Ratio=1613/14876
flop ratio=0.108429685
% of the DFF'S=0.108429685*100=10..84296854%


## Day 1 Summary

On the first day, I became familiar with the complete ASIC design flow and the OpenLANE environment. I successfully explored the Sky130 PDK, understood the directory structure, prepared the design, and executed RTL synthesis. The generated synthesis reports provided valuable information about area, cell usage, and timing characteristics.

# Day 2 – Floorplanning and Placement

## Objective

The second day focused on understanding the physical implementation stage after logic synthesis. The session covered floorplanning, power planning, standard cell placement, and the influence of library cells on chip area and timing.

---

## Overview

After synthesis generates the gate-level netlist, the design enters the physical design stage. During this phase, the logical components are assigned physical locations on the chip while satisfying timing, routing, and area constraints.

The major stages covered include:

- Floorplanning
- Power Planning
- Standard Cell Placement
- Cell Library Characterization

---

# Floorplanning

Floorplanning is the process of defining the chip dimensions and arranging major design blocks before placement begins.

Its primary objectives are:

- Determine chip size
- Define core and die area
- Reserve routing space
- Optimize utilization
- Improve timing performance


# Important Floorplan Parameters

### Utilization Factor

Utilization represents the percentage of core area occupied by standard cells.

A very high utilization leaves little routing space, whereas very low utilization wastes chip area.

Typical utilization values range from **50% to 70%** depending on the design complexity.

---

### Aspect Ratio

Aspect ratio is defined as:

Aspect Ratio = Height / Width

A value of **1** represents a square core.

Different aspect ratios may be selected depending on routing and timing requirements.

---

## Running Floorplan

The floorplanning stage is executed using the following command:

```tcl
run_floorplan
```

This command creates:

- Core area
- Die area
- Standard cell rows
- Placement region
- Power distribution planning

**Screenshot**

<img width="1280" height="768" alt="Screenshot from 2026-07-13 17-11-09" src="https://github.com/user-attachments/assets/35f8935b-b198-4799-842b-71c9eacb0ccd" />

<img width="1280" height="768" alt="Screenshot from 2026-07-13 17-12-14" src="https://github.com/user-attachments/assets/001c31fd-8f1d-439a-a36f-63c727938a5a" />

---

# Floorplan Output Files

After successful execution, OpenLANE generates several important files.

These include:

- DEF file
- Configuration files
- Floorplan reports
- Log files

The DEF file stores the physical layout information generated during floorplanning.

---

# Viewing the Floorplan in Magic

The generated DEF file can be visualized using Magic.

Example command:

```bash
magic -T sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
<img width="1366" height="768" alt="Screenshot from 2026-07-14 13-12-50" src="https://github.com/user-attachments/assets/6c67425f-ea5f-4421-8aa3-42aedcd26db1" />

Magic displays:

- Chip boundary
- Core boundary
- Standard cell rows
- IO pins
<img width="1280" height="768" alt="Screenshot from 2026-07-13 17-31-05" src="https://github.com/user-attachments/assets/e00ae630-4f4d-42f8-86a5-08026a880c4c" />

**Screenshot**

<img width="1366" height="768" alt="Screenshot from 2026-07-13 19-37-20" src="https://github.com/user-attachments/assets/1959f4f7-b894-4f7b-9273-e7e6a9a6a90f" />
<img width="1280" height="768" alt="Screenshot from 2026-07-13 17-34-26" src="https://github.com/user-attachments/assets/ce3dbec1-a4ec-4c1f-8dcf-b13f97bf7e3a" />


---

# Power Planning

Power planning ensures reliable power delivery across the entire chip.

It involves creating:

- VDD rails
- Ground rails
- Power straps
- Power rings

A proper power network minimizes voltage drop and improves circuit stability.

**Screenshot**

![Power Planning](Paste your screenshot here)

---

# Standard Cell Placement

Placement assigns every standard cell to a legal position inside the core.

The objectives are:

- Reduce wirelength
- Improve timing
- Avoid cell overlap
- Optimize routing resources

OpenLANE automatically performs placement after floorplanning.

Command:

```tcl
run_placement
```

**Screenshot**

![Placement Result](Paste your screenshot here)

---

# Viewing Placement in Magic

The placement DEF file can be opened in Magic using:

```bash
magic -T sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

The placement view displays:

- Standard cells
- IO pins
- Core boundary

**Screenshot**

![Placement in Magic](Paste your screenshot here)

---

# Cell Library Characterization

Standard cells are designed using different optimization goals.

Common library types include:

- High Density (HD)
- High Speed (HS)
- Low Power (LP)

Each library provides different trade-offs between:

- Area
- Delay
- Power consumption

Selecting the appropriate library depends on the application requirements.

---

# Observations

During this lab I observed that:

- Floorplanning defines the physical structure of the chip.
- Utilization and aspect ratio directly influence placement quality.
- Power planning improves power distribution.
- Placement determines the physical location of all standard cells.
- Magic provides a graphical representation of the generated DEF files.

---

# Conclusion

On Day 2, I learned the complete floorplanning process and understood how the synthesized design is converted into a physical layout. I explored utilization, aspect ratio, power planning, and standard cell placement using OpenLANE and visualized the generated layouts in Magic.

