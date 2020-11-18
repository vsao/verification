# LVS User Manual
# Overview  
For LVS Calibre nmLVS and Calibre nmLVS-H terminologies are used. When there is no possibility of confusion between flat and hierarchical tools, LVS is used instead of Calibre nmLVS/Calibre nmLVS-H.
- Calibre nmLVS performs flat geometric layout versus schematic netlist comparison.
- Calibre nmLVS-H performs hierarchical layout versus schematic SPICE netlist comparison.

# Tool Invocation
Calibre nmDRC and Calibre nmLVS require rule file and design database inputs in certain formats in order to perform verification tasks. The command lines for these tools have a large set of options for configuring the runs.

## Required Input Files
Before you invoke a Calibre Physical Verification tool, certain files must exist.
- Rule File — Standard Verification Rule Format (SVRF) or Tcl Verification Format(TVF) form.
- Layout Databases — A geometric database for Calibre nmDRC applications, flat LVS, and hierarchical connectivity extraction; a layout SPICE netlist for LVS comparison.
- Source Databases — A SPICE netlist used for LVS comparison.




