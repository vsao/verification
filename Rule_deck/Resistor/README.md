# Resistor(poly-resistor- rnp1)
* This folder contains the following subfiles/folders:

|         Inputs        |          Outputs          |
|-----------------------|---------------------------|
| res_test.calibre.db   |   netlist LAYOUT          |
| res_test.src.net      |   netlist SOURCE          |
| rule_file             |   res_test.sp             |
| xt018_1243            |   res_test.lvs.report.ext |
| res_test.rul          |   res_test.lvs.report     |
| res_test_final.rul    |   svdb                    |

## res_test.calibre.db 
* This is the layout database file generated from layout.
* It is generated on running LVS from the GUI mode.
* It can also be generated without running LVS. 
   * In the menu bar, go to `File`->`Export Mask Data`->`GDSII`. 
   * In the pop-up window that appears, in the `to File` section, give the path and the name of the file that you wish to save your layout database information in (here, the file name is: res_test.calibre.db).
   
## res_test.src.net 
* This file contains the netlist of the schematic (poly resistor).
* This is extracted from schematic on running LVS in GUI mode.(Export from schematic view)

## rule_file
* This is the first rule file containing all the specification statements regarding the source, layout path, lvs and erc report.
* The file points to the second rule file.

## xt018_1243
* This is the second rule file where all modules are defined or undefined.
* It points to t5he third or the main rule file.

## res_test.rul
* This is the main file containing all the information rquired to netlist out the layout and perform LVS.
* It contains a wider rule file view for rnp1.(1st attempt)

## res_test_final.rul
* This is the compact rule file required to netlist and perform LVS correctly for rnp1 only.

All the above input files are put together in a folder. Then the following commands are executed in this path:-
1. ` calibre -spice res_test.sp rule_file` generates the following output file:

## res_test.sp
* This is the spice file that is generated from layout.

2. `calibre -lvs -hier rule_file` generates the following output files:

## netlist LAYOUT
* This is the extracted netlist of layout after LVS Comparison.

## netlist SOURCE
* This is the extracted netlist of source aftewr LVS Comparison.

## res_test.lvs.report.ext
* This file contains the info of LVS report (date,time, path, correct, incorrect)

## res_test.lvs.report
* This file contains the full information of the LVS Comparison.

## svdb
* This is a directory created for storing data files and directories created by Calibre nmLVS.
