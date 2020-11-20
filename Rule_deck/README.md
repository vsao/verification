# LVS User Manual
## Overview  
The Calibre Verification applications operate on rule files written in Standard Verification Rule Format(SVRF) or TCL verification Format(TVF). This mannual will guide one throgh the calibre LVS verification steps.

## LVS Data Flow  
![LVS FLOW](https://github.com/vsao/verification/blob/main/Rule_deck/lvs_flow.png?raw=true)  
Figure1: LVS Data Flow

## Tool Invocation
Calibre nmDRC and Calibre nmLVS require rule file and design database inputs in certain formats in order to perform verification tasks. There are certain files that needed before invoking Calibre Physical Verification tool:
1. Rule File
2. Layout Databases
3. Source Databases 

### Rule File
It is a tandard Verification Rule Format (SVRF) or Tcl Verification Format(TVF) form. Rule files contain two main categories of commands:  
1. Specification statements: Such as describing the layout and source databases, and specifying where to store the results of a run.
2. Layer operations: manipulation of layers through Boolean operations, measurement operations etc.  

- Rule file that is used while running the lvs in command line is inter-linked with some other DRC/LVS Verification rule files as shown below:  

![Rule files Inter-linkage](https://github.com/vsao/verification/blob/main/Rule_deck/rulefile.png)  
Figure2: Rule Files Inter-linkage

- For the reference purpose rule file template can be accessed from the link given below:  
[Rule file Template](https://github.com/vsao/verification/blob/main/Rule_deck/_xt018_1243_)

### Layout Databases
The layout database must be one of the system formats: GDSII, OASIS, LEFDEF, MIlKYWAY, ASCII, Binary, SPICE, CNET

In SPICE Format for Layout, Calibre nmLVS-H netlist-to-netlist comparison uses a SPICE or HSPICE netlist for the layout.
In CNET Format for Layout, CNET stands for Compiled NETlist, a proprietary Mentor Graphics netlist format. This database type can be used for the layout in flat LVS comparison.

### Source Databases
A source database contains circuit information such as schematic netlist or source netlist. The source file is compared to the layout during an LVS run.

### Calibre nmLVS and Calibre nmLVS-H Command Line
- General Command 
```bash
calibre [ -lvs [ [ { -tl || -ts } cnet_file_name ][ -nonames ] [ -cell ][ -dblayers "name1,..." ][ -bpf [ no-extents ] ] [ -nl ] [ -cb ]] || [ -hier [ -automatch || -genhcells[=qs_tcl_file_name] ] || -flatten][ -ixf ] [ -nxf ]]
```
- Spice extraction using command line 
```bash
calibre -spice cellname.sp rulefile
```
- For an example spice extraction of poly-resistor(rnp1) is shown below:  
```bash
/CAD/mentor/calibre/2020-2-14-12/aoi_cal_2020.2_14.12/bin/calibre -spice my_resister.sp _xt018_1243_
 ```
-   ``/CAD/mentor/calibre/2020-2-14-12/aoi_cal_2020.2_14.12/bin/calibre`` is the path of calibre executable file.
-   ``-spice`` command extracts spice netlist from layout file.
-   ``my_resister.sp`` is the name of the extracted spice file.
-   ``_xt018_1243_`` is the rule file.  

- lvs comparison using command line
```bash
calibre -lvs -hier rulefile
```
- For an example lvs comparison of poly-register(rnp1) is shown below:  
```bash
/CAD/mentor/calibre/2020-2-14-12/aoi_cal_2020.2_14.12/bin/calibre -lvs -hier _xt018_1243_
```
- ``/CAD/mentor/calibre/2020-2-14-12/aoi_cal_2020.2_14.12/bin/calibre`` is the path of the calibre executable file.
- ``-lvs`` command performs the lvs comaprison between spice netlists of schematic and layout.
- ``-hier`` command performs herarchical lvs comparision.
- ``_xt018_1243_`` is the rule file.  

- Both spice extraction and lvs comparison can be done using a single command
```bash
/CAD/mentor/calibre/2020-2-14-12/aoi_cal_2020.2_14.12/bin/calibre -spice my_resister.sp -lvs -hier -nowait _xt018_1243_
```
- ``-nowait`` command causes Calibre to wait approximately 10 seconds before attempting to acquire substitute licenses.  

### Output files
By running the command for spice extraction and lvs comparison of a poly-resistor we get certain output files:  
1. ``my_resistor.sp``           : Contains extracted spice netlist of layout named "my_resistor".
```bash
.SUBCKT my_resistor t2 t1  
** N=4 EP=2 IP=0 FDC=1  
R0 t2 t1 L=1e-05 W=2e-06 $[rnp1] $X=-1525 $Y=-1025 $D=122  
.ENDS  
 ```
2. ``netlistLAYOUT``           : Contains the same informations as in my_register.sp
```bash
.SUBCKT my_resistor T2 T1
RR0 T2 T1 RNP1 l=1e-05 w=2e-06
.ENDS
```
3. ``netlistSOURCE``           : Contains extracted spice netlist of schmatic.
```bash
.SUBCKT my_resistor T1 T2
RRR1 T1 T2 RNP1 l=1e-05 w=2e-06
.ENDS
```
4. ``my_resistor.lvs.report.ext``  : Contains the extraction informations such as lvs report file name, layout time, creation time and current directory.  

5. ``my_resistor.lvs.report``  : Contains results of lvs comparison such as ports, nets and instance mismatch along with extraction informations as described in my_resistor.lvs.report.ext.













