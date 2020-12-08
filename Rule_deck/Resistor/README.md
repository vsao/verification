# Resistor(poly-resistor- rnp1)
* This folder contains the following subfiles/folders:

|         Inputs           |          Outputs             |
|--------------------------|------------------------------|
| my_resister.calibre.db   |   netlist LAYOUT             |
| my_resister.src.net      |   netlist SOURCE             |
| inputregrule             |   my_resister.sp             |
| xt018_1243               |   my_resister.lvs.report.ext |
| regrule.rul              |   my_resister.lvs.report     |
| regrul_mod.rul           |   svdb                       |

## my_resister.calibre.db 
* This is the layout database file generated from layout.
* It is generated on running LVS from the GUI mode.
* It can also be generated without running LVS. 
   * In the menu bar, go to `File`->`Export Mask Data`->`GDSII`. 
   * In the pop-up window that appears, in the `to File` section, give the path and the name of the file that you wish to save your layout database information in (here, the file name is: my_resister.calibre.db).
   
## my_resister.src.net 
* This file contains the netlist of the schematic (poly resistor).
* This is extracted from schematic on running LVS in GUI mode.(Export from schematic view)

## inputregrule
* This is the first rule file containing all the specification statements regarding the source, layout path, lvs and erc report.
* The file points to the second rule file.

## xt018_1243
* This is the second rule file where all modules are defined or undefined.
* It points to t5he third or the main rule file.

## regrule.rul
* This is the main file containing all the information rquired to netlist out the layout and perform LVS.
* It contains a wider rule file view for rnp1.(1st attempt)

All the above input files are put together in a folder. Then the following commands are executed in this path:-
1. ` calibre -spice my_resister.sp inputregrule` generates the following output file:

## regrule_mod.rul 
* Same as `regrule.rul` except macro definition. Here perimeter function is used to get W & L of a resistor insted of area.

## my_resister.sp
* This is the spice file that is generated from layout.

2. `calibre -lvs -hier inputregrule` generates the following output files:

## netlist LAYOUT
* This is the extracted netlist of layout after LVS Comparison.

## netlist SOURCE
* This is the extracted netlist of source aftewr LVS Comparison.

## my_resister.lvs.report.ext
* This file contains the info of LVS report (date,time, path, correct, incorrect)

## my_resister.lvs.report
* This file contains the full information of the LVS Comparison.

## svdb
* This is a directory created for storing data files and directories created by Calibre nmLVS.
