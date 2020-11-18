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












