# LVS User Manual
# Overview  
For LVS Calibre nmLVS and Calibre nmLVS-H terminologies are used. When there is no possibility of confusion between flat and hierarchical tools, LVS is used instead of Calibre nmLVS/Calibre nmLVS-H.
- Calibre nmLVS performs flat geometric layout versus schematic netlist comparison.
- Calibre nmLVS-H performs hierarchical layout versus schematic SPICE netlist comparison.

# Tool Invocation
Calibre nmDRC and Calibre nmLVS require rule file and design database inputs in certain formats in order to perform verification tasks. The command lines for these tools have a large set of options for configuring the runs.

## Required Input Files
Files needed before invoking Calibre Physical Verification tool:
- Rule File
- Layout Databases — A geometric database for Calibre nmDRC applications, flat LVS, and hierarchical connectivity extraction; a layout SPICE netlist for LVS comparison.
- Source Databases — A schematic SPICE netlist used for LVS comparison.  

### Rule File
It is a tandard Verification Rule Format (SVRF) or Tcl Verification Format(TVF) form. Rule files contain two main categories of commands:  
1. Specification statements: Such as describing the layout and source databases, and specifying where to store the results of a run.
2. Layer operations: manipulation of layers through Boolean operations, measurement operations etc.  

Required LVS Rule File Statements:
- LAYOUT SYSTEM GDSII
- LAYOUT PATH "<path_name>"
- LAYOUT PRIMARY "<primary_cell_name>"
- SOURCE SYSTEM SPICE
- SOURCE PATH "<path_name>"
- SOURCE PRIMARY "<subckt_cell_name>"

### Layout Databases
The layout database must be one of the system formats:
- GDSII, OASIS, LEFDEF, MIlKYWAY, ASCII, Binary, SPICE, CNET

In SPICE Format for Layout, Calibre nmLVS-H netlist-to-netlist comparison uses a SPICE or HSPICE netlist for the layout.
In CNET Format for Layout, CNET stands for Compiled NETlist, a proprietary Mentor Graphics netlist format. This database type can be used for the layout in flat LVS comparison.

### Source Databases
A source database contains circuit information such as schematic netlist or source netlist. The source file is compared to the layout during an LVS run.

### Calibre nmLVS and Calibre nmLVS-H Command Line
**General Command**  
calibre {-lvs | -perc} [ -cs ] [ -cl ] [ -nowait | -wait n ] [ -cb ] [ -lmconfig licensing_config_filename ] [ -E svrf_output_from_tvf ] rule_file_name

**Spice extraction using command line:**  

**Command for spice extraction of layout:** calibre -spice cellname.sp rulefile

For an example spice extraction of poly-register(rnp1) is shown below:  

**command:**  /CAD/mentor/calibre/2020-2-14-12/aoi_cal_2020.2_14.12/bin/calibre -lvs -hier /home/NIS/projects/XT018-19/A0/work/tt18-vsao/LVS/_xt018_1243_

**Generated output(my_register.sp):**  

.SUBCKT my_register t2 t1  
** N=4 EP=2 IP=0 FDC=1  
R0 t2 t1 L=1e-05 W=2e-06 $[rnp1] $X=-1525 $Y=-1025 $D=122  
.ENDS  

**lvs comparison using command line:**   
**clibre -lvs -hier rulefile**

















