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

![OpenLANE Directory](images/day1/openlane_directory.png)

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

![PDK Structure](images/day1/pdk_structure.png)

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

## Design Preparation

Prepare the design before running synthesis.

```tcl
prep -design picorv32a
```

This command creates the working directory and loads all configuration files needed for implementation.

**Screenshot**

![Preparation](images/day1/prep_design.png)

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

![Synthesis Output](images/day1/synthesis.png)

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

## Day 1 Summary

On the first day, I became familiar with the complete ASIC design flow and the OpenLANE environment. I successfully explored the Sky130 PDK, understood the directory structure, prepared the design, and executed RTL synthesis. The generated synthesis reports provided valuable information about area, cell usage, and timing characteristics.

---
