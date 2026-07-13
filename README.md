# soc-design-and-planning-vsd
RTL-to-GDSII physical design flow using OpenLANE &amp; Sky130 PDK | VSD SoC Design and Planning Workshop.
Digital VLSI SoC Design and Planning — RTL to GDSII

    A 2-week hands-on workshop on complete RTL-to-GDSII flow for digital VLSI SoC design, organised by VSD (VLSI System Design) in collaboration with NASSCOM. This repository documents my learning, lab outputs, and key takeaways from each day.

Day 1 — Inception of Open-Source EDA, OpenLANE & Sky130 PDK
Understanding the Chip Package

When we look at any embedded board and point to what we call the "chip," we're actually looking at the package — a protective casing around the actual silicon die. The real chip sits in the centre of this package and communicates with the outside world via wire bonding — tiny wires that connect the chip's pads to the package pins.
Inside the Chip: Core, Pads, and Die

Zooming into the chip itself, all signals between the chip and the external world pass through pads placed around the periphery. The region enclosed by the pads is called the core — this is where all the actual digital logic lives. Together, the core and the pads form the die, which is the fundamental unit of chip manufacturing.

    Foundry — the place where chips are physically manufactured
    Foundry IPs — IP blocks that require specialized process knowledge to implement (e.g., PLLs, SRAMs)
    Macros — reusable, purely digital logic blocks

From Software to Silicon — The ISA Bridge

A C program running on a chip goes through a multi-layer transformation:

    The C code is compiled into RISC-V assembly (or another ISA)
    The assembler converts it to binary machine code (0s and 1s)
    This binary pattern needs an RTL implementation of the ISA
    The RTL gets synthesized and goes through the full PnR (Place and Route) flow to become a physical layout

The system software stack (OS → Compiler → Assembler) acts as the bridge between what the programmer writes and what the hardware executes.
Why Open-Source EDA Matters

For a fully open-source ASIC design flow, three things are needed:

    RTL Designs (e.g., from opencores.org)
    EDA Tools (synthesis, P&R, verification)
    PDK Data (process-specific design rules, standard cell libraries)

Historically, PDKs were proprietary and distributed only under NDAs, making chip design inaccessible to most people. This changed in June 2020, when Google collaborated with SkyWater Technology to release the Sky130 PDK as the world's first open-source process design kit — a massive milestone for the VLSI community.
OpenLANE and the Automated RTL to GDSII Flow

OpenLANE is an open-source flow built on top of multiple EDA tools that automates the journey from an RTL netlist all the way to the final GDSII layout file. It uses:
Stage 	Tool(s) Used
Synthesis 	Yosys, ABC
Floorplan & PDN 	OpenROAD
Placement 	OpenROAD
CTS 	TritonCTS
Routing 	FastRoute, TritonRoute
SPEF Extraction 	OpenRCX
GDS Streaming 	Magic, KLayout
Timing Analysis 	OpenSTA
DRC & LVS 	Magic, Netgen
Lab — Running OpenLANE for picorv32a
Setting Up and Invoking OpenLANE

The very first step is to navigate to the OpenLANE working directory and launch the tool in interactive mode, which lets us run each stage step-by-step.
