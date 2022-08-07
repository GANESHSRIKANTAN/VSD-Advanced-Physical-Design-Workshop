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


# Day 2 :- Good floorplan vs bad floorplan and introduction to library cells 

Floorplanning involves determining the locations, shape, size of modules in a chip and as such it estimates the chip area, delay and the wiring congestion, thereby providing a ground work for layout

Each chip has a die area and a core area. Die area encapsulates core area.

utilisation is defined as the ratio of the area occupied by the netlist / the area occupied by the core 

Aspect ratio is defined as the ratio of the height of the chip and the width of the chip 

There are pre-placed chips in every circuit designs ( for examples SDRAMS, clocks , Adders etc ) 

These have a specific location and size in the chip - not to be messed with 


floor planning considerations :- 

1. IPs 2. Decoupling capacitors 3. Power rails 4. pins and its placement 

![Screenshot (52)](https://user-images.githubusercontent.com/19291332/183238519-d932e1e7-dabd-4148-831a-ffa4cd62f12e.png)


Based on the diagram above we need to find an optimal floorplan 

WE run the floor plan using the command 

' run_floorplan' 

in the docker window 

![Screenshot (53)](https://user-images.githubusercontent.com/19291332/183238624-327483b6-96aa-4fdf-b698-2b94ad67588e.png)


this is the output we get 

![Screenshot (54)](https://user-images.githubusercontent.com/19291332/183238644-bbd89ba2-0384-4fbb-9198-90a122fa44f7.png)

![Screenshot (55)](https://user-images.githubusercontent.com/19291332/183238778-f4e3531c-41a5-40f0-82de-f372142dc647.png)




after running the command 

'magic -T /home/iganesh/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &'

Place & Route :-------------

library consists of size, delay of the cell and requirement of each cell .
For signal integrity we use cell repeaters 

Final step is Static timing analysis

![Screenshot (57)](https://user-images.githubusercontent.com/19291332/183238836-83b668f9-78b1-40d7-8f0c-416e1731594e.png)

# Day 3 :- Design library cell using Magic Layout and ngspice characterization

In day 3, we learn how we can make changes to the floor plan and modify the spacing of the IO pins in the chip on the fly 

For exammple - To change the spacing of the io pins we use 

                                      set ::env( FP_IO_MODE) 2
                                      
To change the spacing from equidistant to crowded 

## SPICE Simulation :- 

We are using ngspice to perform simulation for a cmos inverter. 
The aim of the lab exercise is to find out the rise and fall times 

### ngSPICE :- 

We describe the netlist and the models reqired to simulate an cmos inverter 

```

* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale= 0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib





M1 Y A VPWR VPWR pshort_model.0 ad=0 pd=0 ps=0 w=35 l=23
M2 Y A VGND VGND nshort_model.0 ad=0 pd=0 ps=0 w=37 l=23

VDD VPWR 0 3.3V
VSS VGND 0 0V

VA A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)
C0 A Y 0.05fF
C1 Y VPWR 0.11fF
C2 A VPWR 0.07fF
C3 y  VGND 2fF
C4 VPWR VGND 0.59fF

.TRAN 1n 20n
.control
run
.endc
.end

```

To obtain the netlist for an inverter, we do the following 
1. Firstly clone the repository https://github.com/nickson-jose/vsdstdcelldesign.git
                     
2. open magic by using the command

``` magic ``` 

in the terminal.

3. Open file 'sky130_inv.mag' , you will get the layout like this 
![Screenshot (63)](https://user-images.githubusercontent.com/19291332/183274036-aaf813bc-27f9-4cb2-bfff-da0002ad6e05.png)



    








