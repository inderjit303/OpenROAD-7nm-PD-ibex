# OpenROAD Flow
- OpenROAD Flow is a full RTL-to-GDS flow built entirely on open-source tools.
- The project aims for automated, no-human-in-the-loop digital circuit design with 24-hour turnaround time.

## Using the Flow

- See the OpenROAD [documentation here](https://openroad.readthedocs.io/en/latest/).
- How to [start using OpenROAD flow here](https://openroad-flow-scripts.readthedocs.io/en/latest/user/GettingStarted.html).
- Our [user guide here](https://openroad-flow-scripts.readthedocs.io/en/latest/user/UserGuide.html).
- Our [Flow Tutorial here](https://openroad-flow-scripts.readthedocs.io/en/latest/tutorials/FlowTutorial.html).


# The OpenROAD 7nm Physical Design Contest

## About OpenROAD contest
- OpenROAD delivers open-source and barrier-free VLSI Solutions for RTL-GDSII flow implementation for hardware and software design engineers, enthusiasts and researchers.

- This contest, organized by The OpenROAD Project and VSD, features interesting design challenges at an advanced technology node (7nm).

## Design Goals
The contest is aimed at significantly improving area, performance and Runtime for an opensource RISC-V Core using OpenROAD-Flow-Scripts on an advanced technology node 7nm using ASAP7 PDK.

### Problem A: Best Performance
Using any of the following RISC-V cores from the OpenROAD-flow-scripts repository: RISCV32i, ibex, swerv_wrapper demonstrate the best achievable performance for the design for a given die size on ASAP7. The design must be DRC and LVS clean.

### Problem B: Best Possible Runtime
Using any of the following RISC-V cores from the OpenROAD flow-scripts repository: RISC-V32i, ibex, swerv_wrapper demonstrate the fastest Runtime from RTL-GDSII with good area and performance. Use cloud resources, suitable design configurations, tool changes (any or all of these) to meet this target. The design must be DRC and LVS clean.

# Enter OpenROAD 7nm PD Contest
- This repository is designed to share my learnings and work during the duration of the contest and beyond. 

- The best guide can be found at the OpenROAD Flow Scripts tutorial [here](https://openroad-flow-scripts.readthedocs.io/en/latest/tutorials/FlowTutorial.html#introduction)

- My plan is to break the contest into two parts, 
- **Part 1**: Understanding the ORFS flow on existing RISC-V ibex core using ASAP7 PDK
- **Part 2**: Work on tool improvement i.e Improving the best achievable performance for the design for a ibex core on ASAP7

# **Problem statement for Part 2:** 
1. To use AutoTuner, an automatic RTL to-GDS hyperparameter tuning framework for OpenROAD-flow-script (ORFS) that leverages the METRICS2.1 infrastructure to meet PPA design goals. Will go for Best performance i.e solving Problem A. Best parameters generated will be fed to config.mk to optimize the performance. 
2. To work on flow control parameters in config.mk file aim to reduce the congestion in GDS2 layout in OpenROAD GUI.

# Part 1: OpenROAD Flow Scripts (ORFS) tool usuage Experience 

## OpenROAD & ORFS

- **OpenROAD** is an integrated chip physical design tool that takes a design from from RTL to GDSII, including synthesis, floorplanning, placement, routing, signoff parasitic extraction and timing analysis.

- It uses a hierarchical placement algorithm that aims to minimize wire length, and it provides several features to optimize timing and power consumption. OpenROAD is designed to be extensible and customizable, with a flexible architecture that allows users to add their own algorithms and features.

- OpenROAD is a foundational building block in open-source digital flows like OpenROAD-flow-scripts , OpenLane from Efabless , Silicon Compiler Systems; as well as OpenFASoC for mixed-signal design flows.

- **OpenROAD-flow-scripts(ORFS)** is a flow controller that provides a collection of open-source tools for automated digital ASIC design from synthesis to layout. It provides a fully automated RTL-to-GDSII design flow, which includes Synthesis, Placement and Routing (pnr), Static Timing Analysis (sta), Design Rule Check (drc) and Layout Versus Schematic (lvs) checks. 

![rtlgds](https://user-images.githubusercontent.com/99788755/227723747-42d1ff3f-a86a-4d03-bf82-e6f5a8cb4ee2.png)

- In ORFS, OpenROAD is used as a plugin for the physical design stage, and it can be configured and customized to meet the specific needs of the design project. The OpenROAD plugin in ORFS provides access to several advanced features, such as hierarchical placement, global routing, and detailed routing optimization.

- ORFS supports several public open sourcePDKs. Available public PDK's are GF180, Skywater130, ASAP7 etc.

- More about the OpenROAD Project can be found [here](https://openroad.readthedocs.io/en/latest/main/README.html)

## Installing and setting up ORFS

- The best resource for setting up the toolchain can be found [here](https://openroad-flow-scripts.readthedocs.io/en/latest/user/BuildLocally.html). A shorter version of the steps  is documented below.

-Clone the following repository to install ORFS
```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
```

-Install dependency using internal script ( It takes care of all the additional packages)
```
cd OpenROAD-flow-scripts
sudo ./etc/DependencyInstaller.sh
```

- Build & Install ( This  installs all the required scripts and the OpenRoad tool as well)
```
./build_openroad.sh --local
```

- Verify Installation

```
source ./setup_env.sh
yosys -help
openroad -help
exit
```
## Steps for RTL to GDS2 for RISC-V ibex core processor and ASAP7 7nm PDK:
- To experience OpenROAD Flow scripts, follow the steps below to generate the GDSII files for the `ibex` RISC V processor and the `ASAP7` PDK.

- More about the ibex RISC V processor can be found [here](https://github.com/lowRISC/ibex)

- Change your current directory to the flow directory.
```
cd flow
```
- Type the following command: 

```
make DESIGN_CONFIG=./designs/asap7/ibex/config.mk` 
```

- Note: this is only for the ibex processor using asap7, other designs will have their other respective design files

 - OpenROAD offers an interactive to analyse the generated GDSII and the gui is launched using

 ```
 make DESIGN_CONFIG=./designs/asap7/ibex/config.mk gui_final
 ```

![image](https://user-images.githubusercontent.com/99788755/227724636-0c4b82be-8f08-462e-bdc8-0a44035243a5.png)

- That is it !! Output gds is created. 

- With a single command OpenROAD convert the synthesize RTL to GDSII. 
- This process takes about 30 minutes
- This builds the ‘ibex‘ 32-bit RISC-V CPU core and the results end up here:
```
OpenROAD-flow-scripts/flow/results/sky130hd/ibex/base$
```

![image](https://user-images.githubusercontent.com/99788755/227724732-c6280df2-b86d-4b86-aaad-2266acb48a6b.png)

## Check MakeFile contents

![image](https://user-images.githubusercontent.com/99788755/227725067-d0eb2fd1-200c-4376-b4c5-2e29382bae95.png)

## Important design/config files 
 - config.mk 
 - autotuner.json
 - constraint.sdc

![image](https://user-images.githubusercontent.com/99788755/227726632-d3f8b12d-01bd-4c8b-844d-90f1c41546b2.png)

## Config.mk file contents

- Contents of config.mk file, which is the default design configuration of ibex core 

- Note: Design flow variables setting with ?= ______ can only be varied. 

![image](https://user-images.githubusercontent.com/99788755/227729218-938cdb6b-c850-4f5f-a81f-786f9959dd26.png)

## constraint.sdc file contents

- The timing constraints are specified in this file 

![image](https://user-images.githubusercontent.com/99788755/227729418-65a5ad3e-39f6-449c-b127-80aa85f08a13.png)

## Synthesis script tcl file contents 

- The OpenROAD Flow starts with a flow configuration file (config.mk), the chosen platform (asap7) and the Verilog files (ibex_core.v) of ibex core. 

- From them, synthesis is run using Yosys to find the appropriate circuit implementation from the available cells in the platform.

![image](https://user-images.githubusercontent.com/99788755/227782586-69ffe878-1564-4c13-a444-a162ade7591d.png)

## Power Grid tcl file 

- Power frid strategy .tcl file contains information on various signal, input and supply power constraints. 

![image](https://user-images.githubusercontent.com/99788755/227783605-d57e9440-90d2-421b-8ed3-2ebb8e721152.png)


## Log files locations 

- Log directory, which contains log files of every stage of ORFS. 
- Below is the image of location and various Log files of ibex core processor using ASAP7 PDK 
- Type the following command to view all logs: 
```
OpenROAD-flow-scripts/flow/logs/asap7/ibex/base
```

![image](https://user-images.githubusercontent.com/99788755/227782282-2f6eb5f9-3135-4b7d-b45f-1b4d6b391ee5.png)

## Yosys.log contents 

![image](https://user-images.githubusercontent.com/99788755/227784952-75d87227-597f-4327-af46-b1aa551dca4b.png)

## Floorplan.log contents 

- Floorplan for the physical design is generated with OpenROAD, which requires a description of the power delivery network (in pdn.tcl).

![image](https://user-images.githubusercontent.com/99788755/227785031-d1bf225b-7ce9-4e74-aeb9-2f85f99661b9.png)

- Within floorplan log file, observe worst negative slack wns, total negative slack tns, worst slack

- Negative Slack (NS): Negative slack is the amount of time by which a signal arrives later than it is required to arrive at a particular point in the circuit. It is calculated as the difference between the required arrival time and the actual arrival time of a signal. A negative slack value indicates that the circuit is not meeting its timing requirements and may result in timing violations.

- Worst Negative Slack (WNS): Worst negative slack (WNS) is the most negative value of the slack across all paths in the circuit. It represents the worst-case timing violation in the design.

- Total Negative Slack (TNS): Total negative slack (TNS) is the sum of all the negative slack values across all paths in the circuit. It represents the overall timing violation in the design.

![image](https://user-images.githubusercontent.com/99788755/227785207-c10766a6-6fab-48ad-bd70-43930d593864.png)

- Floorplan.log file also contains elaborated timing report for the each timing path

![image](https://user-images.githubusercontent.com/99788755/227785284-a683f6cc-af73-47f4-b3f4-97d8ceae1681.png)

## CTS log file contents 

![image](https://user-images.githubusercontent.com/99788755/227786319-84f58f16-c844-46ad-9031-2bd4c9243fa5.png)

## Routing

- Routing is also divided into two phases: global routing and detailed routing. 

### FastRouting log contents (Global routing)

![image](https://user-images.githubusercontent.com/99788755/227786630-0c577ca8-e87d-4d3b-ad6b-71657e87d27c.png)

### Triton.log contents (Detailed routing)

![image](https://user-images.githubusercontent.com/99788755/227786719-a3e9505f-f4cc-4815-b205-67a09da22b1f.png)

![image](https://user-images.githubusercontent.com/99788755/227786814-d63f5024-8de6-4dd2-97d1-2ab4a916b89b.png)

## Merge log contents 

![image](https://user-images.githubusercontent.com/99788755/227786861-30ea1af8-864f-4961-a83c-4a98899c4904.png)

## Final report log contents 

![image](https://user-images.githubusercontent.com/99788755/227787096-081787f9-6280-4757-b5a8-977f6b4a74af.png)

- Timing violations details in final report 

![image](https://user-images.githubusercontent.com/99788755/227787142-2f8f9ad1-3d72-469d-ab55-7a5a9726f58c.png)

## Final Power report

- Final Power report are observed within the final report log file. 

![image](https://user-images.githubusercontent.com/99788755/227787258-c2d4f8d0-e814-4e2c-aa8c-83d0d214d468.png)

- **Sequential Power Usage**: Sequential power usage refers to the power consumed by the flip-flops and latches in a circuit. These elements store the state of the circuit, so they consume power even when the circuit is not actively switching.

- **Combinational Power Usage**: Combinational power usage refers to the power consumed by the logic gates and interconnects in a circuit. These elements do not store any state, so they only consume power when the circuit is actively switching.

- **Macro Power Usage**: Macro power usage refers to the power consumed by large IP blocks or subcircuits in the design. These blocks are typically provided by third-party vendors and can have a significant impact on the overall power consumption of the chip.

- **Pad Power Usage**: Pad power usage refers to the power consumed by the input/output (I/O) pads of the chip. These pads interface the chip with the outside world and are typically designed to meet specific electrical standards.

- **Area and Core Utilization**: Design area and its core utilization is visible at the end of final report log. 

## Final GDS view using klayout

- Final GDS is located as shown in the command below

```
/OpenROAD-flow-scripts/flow/results/asap7/ibex/base
```

![image](https://user-images.githubusercontent.com/99788755/227790627-c293434d-8640-4cc8-a582-8245561d3988.png)


### KLAYOUT VIEW OF 6_final.gds for ibex core using ASAP7 

- Klayout view can be invoked by following command 

```
/OpenROAD-flow-scripts/flow/results/asap7/ibex/base$ klayout 6_final.gds
```

![image](https://user-images.githubusercontent.com/99788755/227791921-e7c8e48e-cd94-4604-881b-8ce947771287.png)

- Expanded klayout view

![image](https://user-images.githubusercontent.com/99788755/227792349-60fa8faa-0b04-4ef1-892b-e8bd1f852b6f.png)


## Checking Reports for final design 

- At the end, ORFS will output its logs under ```flow/reports/```, and its results under ```flow/results/```

### Routing report (route_drc.rpt)

- Since there are no DRC violation in the final ibex core design using ASPA7 7nm PDK, routing report is empty 

![image](https://user-images.githubusercontent.com/99788755/227792660-8a4d3c66-b1ab-4f44-9c28-fe509e8e3156.png)

### Synthesis statistics (synth_stat.txt)

![image](https://user-images.githubusercontent.com/99788755/227792804-d463725b-2db0-4092-9548-210e4cb3bbcd.png)

![image](https://user-images.githubusercontent.com/99788755/227792859-e8650930-e95f-4e09-8fdc-42ebf845d3e0.png)

## OpenROAD GUI exploration: 




# End of Part 1: OpenROAD Flow Scripts (ORFS) tool usuage Experience





































## Citing this Work

If you use this software in any published work, we would appreciate a citation!
Please use the following references:

```
@article{ajayi2019openroad,
  title={OpenROAD: Toward a Self-Driving, Open-Source Digital Layout Implementation Tool Chain},
  author={Ajayi, T and Blaauw, D and Chan, TB and Cheng, CK and Chhabria, VA and Choo, DK and Coltella, M and Dobre, S and Dreslinski, R and Foga{\c{c}}a, M and others},
  journal={Proc. GOMACTECH},
  pages={1105--1110},
  year={2019}
}
```

A copy of this paper is available
[here](http://people.ece.umn.edu/users/sachin/conf/gomactech19.pdf) (PDF).

```
@inproceedings{ajayi2019toward,
  title={Toward an open-source digital flow: First learnings from the openroad project},
  author={Ajayi, Tutu and Chhabria, Vidya A and Foga{\c{c}}a, Mateus and Hashemi, Soheil and Hosny, Abdelrahman and Kahng, Andrew B and Kim, Minsoo and Lee, Jeongsup and Mallappa, Uday and Neseem, Marina and others},
  booktitle={Proceedings of the 56th Annual Design Automation Conference 2019},
  pages={1--4},
  year={2019}
}
```

A copy of this paper is available
[here](https://vlsicad.ucsd.edu/Publications/Conferences/371/c371.pdf) (PDF).

If you like the tools, please give us a star on our GitHub repos!

## License

The OpenROAD-flow-scripts repository (build and run scripts) has a BSD 3-Clause License.
The flow relies on several tools, platforms and designs that each have their own licenses:

- Find the tool license at: `OpenROAD-flow-scripts/tools/{tool}/` or `OpenROAD-flow-scripts/tools/OpenROAD/src/{tool}/`.
- Find the platform license at: `OpenROAD-flow-scripts/flow/platforms/{platform}/`.
- Find the design license at: `OpenROAD-flow-scripts/flow/designs/src/{design}/`.
