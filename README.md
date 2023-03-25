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

# **Problem statement:** 
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

## 













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
