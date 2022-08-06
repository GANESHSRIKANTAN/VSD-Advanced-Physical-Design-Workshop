# VSD-Advanced-Physical-Design-Workshop

This file contains all the labs and Exams that were undertaken for the Advanced Physical Design Workshop conducted by VSD using OpenLane/Sky130 from 03rd Aug 2022 to 07th Aug 2022.

# Day 1 - Getting Familiar with OpenLane
Day 1 lectures introduced us to the basics of VLSI chip design process, beginning with instruction set architecture , RTL and drilling all the way down to placement and routing and GDSII.

Also introduced us to the open source tools available to us such as YoSys,OpenLane, Magic, ngSPICE, openSTA etc. 

The aim of the lab exercise given on Day 1 :- Find out the chip area and the flop ratio after synthesizing picoRV32a using YoSys

The lab instance provided to us has all the necessary tools for us to use for the wrokshop. The tools are at :- 

/Desktop/work/tools/openlane_working_dir/openlane$

RUNNING THE OPENLANE TOOL: 

We start by running the docker daemon service /Desktop/work/tools/openlane_working_dir/openlane$ docker

which should open the bash shell and look something like this:- 

![Screenshot (51)](https://user-images.githubusercontent.com/19291332/183231758-c1fe6658-b916-49c4-977c-b4b0eb2ab2ec.png)

Then we use the command ' ./flow.tcl -interactive ' to run openLANE

![image](https://user-images.githubusercontent.com/19291332/183231862-f1008a92-b018-4381-9084-1a37b7b047f1.png)

We use command ' package require openlane 0.9 ' 

Before we run synthesis, we need to prepare the directories and files 

![Screenshot (28)](https://user-images.githubusercontent.com/19291332/183231964-ea589942-2ee8-410c-ae7e-3f86ed8ad7d6.png)

command :- ' prep -design picorv32a ' 

then we run synthesis :- ' run_synthesis ' 

![Screenshot (37)](https://user-images.githubusercontent.com/19291332/183232011-6756d914-6abe-445a-8185-d4a99334a50a.png)

A sucessful run 

Chip area = 147712.98400 

![Screenshot (36)](https://user-images.githubusercontent.com/19291332/183232043-5c919677-76aa-49ee-8e25-8d80a45932c7.png)

flop ratio = dfxtp / total area = 1613 / 14876 
 
           = 0.10482 = 10.482 %






