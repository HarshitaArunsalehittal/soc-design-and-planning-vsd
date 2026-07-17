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
 Calculate the die area in microns from the values in floorplan def.
 <img width="1280" height="768" alt="Screenshot from 2026-07-13 17-31-05" src="https://github.com/user-attachments/assets/d29fd819-c7e0-42b4-97c5-afe906bc7e29" />
Acoording to flooplan def 
       distance in microns =value in unit distance/1000
       die width in microns =660685/1000=660.685 Microns
       die height in microns= 671405/1000= 671.405 Microns
       Area of die in microns=660.685*671.405=443587.212425 sq microns

## Running Floorplan

The floorplanning stage is executed using the following command:
**Screenshot**

<img width="1280" height="768" alt="Screenshot from 2026-07-13 17-11-09" src="https://github.com/user-attachments/assets/35f8935b-b198-4799-842b-71c9eacb0ccd" />

<img width="1280" height="768" alt="Screenshot from 2026-07-13 17-12-14" src="https://github.com/user-attachments/assets/001c31fd-8f1d-439a-a36f-63c727938a5a" />

```tcl
run_floorplan
```

This command creates:

- Core area
- Die area
- Standard cell rows
- Placement region
- Power distribution planning

 Load generated floorplan def in magic tool and explore the floorplan.
Commands to load floorplan def in magic in another terminal
Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &


<img width="1366" height="768" alt="Screenshot from 2026-07-13 19-37-12" src="https://github.com/user-attachments/assets/9067dc5b-3588-4e96-a548-a211891eeb24" />

<img width="1366" height="768" alt="Screenshot from 2026-07-13 19-37-20" src="https://github.com/user-attachments/assets/7168b486-e49f-4d95-addf-562bcc23656b" />

 Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

 Congestion aware placement by default
run_placement

<img width="1366" height="768" alt="Screenshot from 2026-07-13 22-53-35" src="https://github.com/user-attachments/assets/142b9358-f12a-4f07-a22f-33d6fead18c2" />

. Load generated placement def in magic tool and explore the placement.
Commands to load placement def in magic in another terminal

Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

 Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
Screenshots of floorplan def in magic


<img width="1366" height="768" alt="Screenshot from 2026-07-14 11-43-18" src="https://github.com/user-attachments/assets/7b536f4c-8a81-4314-8078-f54d2517ecda" />
On Day 2, I learned the complete floorplanning process and understood how the synthesized design is converted into a physical layout. I explored utilization, aspect ratio, power planning, and standard cell placement using OpenLANE and visualized the generated layouts in Magic.

 Day 3 Design library cell using Magic Layout and ngspice characterization 

labs
. Clone custom inverter standard cell design from github repository

# Day 3 Design library cell using Magic Layout and ngspice characterization
 Clone custom inverter standard cell design from github repository

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

Change into repository directory
```bash
cd vsdstdcelldesign
```
Copy magic tech file to the repo directory for easy access
```bash
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
```
Check contents whether everything is present
```bash
ls
```
Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
<img width="1366" height="768" alt="Screenshot 2026-07-15 105547" src="https://github.com/user-attachments/assets/50c25333-ffa5-45c9-980f-8c647f1b6796" />
 Load the custom inverter layout in magic and explore
 Screenshot of custom inverter layout in magic
 <img width="1366" height="662" alt="Screenshot from 2026-07-17 12-26-28" src="https://github.com/user-attachments/assets/a66eb718-644a-46b6-b431-6c2e7d6f6c6f" />


 Spice extraction of inverter in magic.
 Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic
Check current directory
```bash
pwd
```
Extraction command to extract to .ext format
```bash
extract all
```

Before converting ext to spice this command enable the parasitic extraction also
```bash
ext2spice cthresh 0 rthresh 0
```
Converting to ext to spice
```bash
ext2spice
```
<img width="1366" height="768" alt="Screenshot 2026-07-14 183901" src="https://github.com/user-attachments/assets/247cce47-91ed-4431-9426-242122bbafc0" />

Screenshot of created spice file
<img width="1366" height="768" alt="Screenshot 2026-07-14 184037" src="https://github.com/user-attachments/assets/867be972-3110-4752-b147-cab00a13046e" />
 Editing the spice model file for analysis through simulation.
Measuring unit distance in layout grid
<img width="1366" height="768" alt="Screenshot 2026-07-15 164054" src="https://github.com/user-attachments/assets/0a6267ce-1133-4959-980e-c7fc00758ef5" />

<img width="1366" height="768" alt="Screenshot 2026-07-14 184743" src="https://github.com/user-attachments/assets/42f4a107-97eb-48ac-9e1b-adee273796a0" />

Final edited spice file ready for ngspice simulation
<img width="1366" height="662" alt="Screenshot from 2026-07-17 13-06-10" src="https://github.com/user-attachments/assets/a40d1185-0fe0-4777-b2e9-3b92b8e97bd2" />

Post-layout ngspice simulations.
Commands for ngspice simulation
Command to directly load spice file for simulation to ngspice
```bash
ngspice sky130_inv.spice
```
Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
```bash
plot y vs time a
```
Screenshots of ngspice run
<img width="1366" height="768" alt="Screenshot 2026-07-15 171539" src="https://github.com/user-attachments/assets/934af055-4b9b-44f8-b5e2-4a90e49bcf9f" />
<img width="1366" height="768" alt="Screenshot 2026-07-15 172513" src="https://github.com/user-attachments/assets/3a1f20bf-a67e-4229-bfae-10c9860634c2" />
<img width="1366" height="768" alt="Screenshot 2026-07-15 173250" src="https://github.com/user-attachments/assets/112c5825-556a-40fa-80b0-6d5ec79412d4" />
<img width="1366" height="768" alt="Screenshot 2026-07-15 173341" src="https://github.com/user-attachments/assets/29b03a43-ab80-4c1a-9ede-5b349326f504" />
Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

Change to home directory
```bash
cd

Command to download the lab files
```bash
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
Since lab file is compressed command to extract it
```bash
tar xfz drc_tests.tgz
```Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u
Change directory into the lab folder
```bash
cd drc_tests
```
List all files and directories present in the current directory
```bash
ls -ltr
```
Command to open magic tool in better graphics
```bash
magic -d XR &
```
<img width="1280" height="768" alt="Screenshot from 2026-07-15 19-50-11" src="https://github.com/user-attachments/assets/f2994fcf-7091-49f7-9132-8729bfedffd0" />
<img width="1280" height="768" alt="Screenshot from 2026-07-15 19-50-22" src="https://github.com/user-attachments/assets/b73c07e7-d4a3-4adc-9691-e1fecf1e0a52" />
<img width="1280" height="768" alt="Screenshot from 2026-07-15 19-50-29" src="https://github.com/user-attachments/assets/5c47c3d3-124e-4823-b8f2-b7fa838288f9" />
<img width="1280" height="768" alt="Screenshot from 2026-07-15 20-06-28" src="https://github.com/user-attachments/assets/315da798-ea1c-4b46-9cf7-19e93093ad86" />
<img width="1366" height="662" alt="Screenshot from 2026-07-15 20-58-54" src="https://github.com/user-attachments/assets/de8815fe-3180-4449-924f-3ca49da2af25" />


<img width="1366" height="662" alt="Screenshot from 2026-07-15 21-35-40" src="https://github.com/user-attachments/assets/ccaf29e7-03b7-4fee-b286-474b018d0305" />
<img width="1366" height="662" alt="Screenshot from 2026-07-15 21-38-25" src="https://github.com/user-attachments/assets/30e8153b-57ce-4983-b5b6-ebff601fb101" />
<img width="1366" height="662" alt="Screenshot from 2026-07-15 22-01-46" src="https://github.com/user-attachments/assets/4b5954ba-137b-4969-a9f4-c3f9843dd6f7" />

# Day 4  Pre-layout timing analysis and importance of good clock tree 

Commands to open the custom inverter layout
 Change directory to vsdstdcelldesign
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```
Command to open custom inverter layout in magic
```bash
magic -T sky130A.tech sky130_inv.mag &
```
<img width="1366" height="662" alt="Screenshot from 2026-07-16 11-19-34" src="https://github.com/user-attachments/assets/0998102b-dc41-4baf-bf73-01bd429c39ea" />


Commands for tkcon window to set grid as tracks of locali layer
Get syntax for grid command
```bash
help grid
```
Set grid values accordingly
```bash
grid 0.46um 0.34um 0.23um 0.17um
```
<img width="1366" height="662" alt="Screenshot from 2026-07-16 11-27-06" src="https://github.com/user-attachments/assets/5e96bb1c-c010-4f6b-911d-1b48e13c2900" />
<img width="1366" height="662" alt="Screenshot from 2026-07-16 11-26-37" src="https://github.com/user-attachments/assets/46842ea6-0f47-4968-afcb-9459e3273f72" />

 Save the finalized layout with custom name and open it.
 Command for tkcon window to save the layout with custom name
Command to save as
```bash
save sky130_vsdinv.mag
```
<img width="1280" height="768" alt="Screenshot from 2026-07-16 12-36-59" src="https://github.com/user-attachments/assets/a2f6a5db-9e01-4cee-b98c-02a838523758" />

Generate lef from the layout
 Command for tkcon window to write lef
 lef command
 ```bash
lef write
```
<img width="1280" height="768" alt="Screenshot from 2026-07-16 12-41-33" src="https://github.com/user-attachments/assets/6acda66f-ec6f-4f91-a706-28860f6be8f1" />

Run openlane flow synthesis with newly inserted custom inverter cell
Commands to invoke the OpenLANE flow include new lef and perform synthesis

Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane
docker

Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 13-07_17-15 -overwrite

Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

#Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
<img width="1280" height="768" alt="Screenshot from 2026-07-16 13-19-27" src="https://github.com/user-attachments/assets/c5b3551f-c928-434e-93c4-d68a13031bd1" />
<img width="1280" height="768" alt="Screenshot from 2026-07-16 14-42-05" src="https://github.com/user-attachments/assets/cfde78c8-6eca-4372-85cb-e99e2d69f7a4" />
<img width="1280" height="768" alt="Screenshot from 2026-07-16 16-04-04" src="https://github.com/user-attachments/assets/07a85dbb-7264-48f2-9ddd-333f0eacabc9" />

Now we are ready to run placement
run_placement

Commands to load placement def in magic in another terminal

Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/

Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
Screenshot of placement def in magic
<img width="1280" height="768" alt="Screenshot from 2026-07-16 16-13-41" src="https://github.com/user-attachments/assets/d1cc3e7c-92c2-4705-890e-7f371ba6f3ae" />
<img width="1280" height="768" alt="Screenshot from 2026-07-16 16-18-09" src="https://github.com/user-attachments/assets/84b0c9c2-71db-4871-8349-1346590ba375" />
Command for tkcon window to view internal layers of cells

Command to view internal connectivity layers
 ```bash
expand
```
Abutment of power pins with other cell from library clearly visible
<img width="1280" height="768" alt="Screenshot from 2026-07-16 16-29-03" src="https://github.com/user-attachments/assets/241fa0be-b157-4c37-a953-50a163e70248" />
