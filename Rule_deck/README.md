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
It is a standard Verification Rule Format (SVRF) or Tcl Verification Format(TVF) form. Rule files contain two main categories of commands:  
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
calibre -spice my_resister.sp _xt018_1243_
 ```
-   ``calibre``is the calibre executable file.
-   ``-spice`` command extracts spice netlist from layout file.
-   ``my_resister.sp`` is the name of the extracted spice file.
-   ``_xt018_1243_`` is the rule file.  

- lvs comparison using command line
```bash
calibre -lvs -hier rulefile
```
- For an example lvs comparison of poly-register(rnp1) is shown below:  
```bash
calibre -lvs -hier _xt018_1243_
```
- ``calibre`` is the calibre executable file.
- ``-lvs`` command performs the lvs comaprison between spice netlists of schematic and layout.
- ``-hier`` command performs herarchical lvs comparision.
- ``_xt018_1243_`` is the rule file.  

- Both spice extraction and lvs comparison can be done using a single command
```bash
calibre -spice my_resister.sp -lvs -hier -nowait _xt018_1243_
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


## Summary of Rule File Elements
This Section provides summary information on the SVRV commands used to write a rule file for Poly-resistor(rnp1).
- **Layer**: This statement specifies input layer names and numbers to be used in the rule file.  
             Syntax: `Layer <name> <original_layer>`  
  - `name`: Specifies the name of the layer. Each name must be unique.
  - `original_layer`: It can be a layer number(allowed from 0 to 131071) or a layer name.  

Examples:  
 1. `layer poly_dg` : layer statement to assign name of the layer as poly_dg
 2. `Layer MAP 13 DATATYPE 0 5013` : 
    - `Layer MAP` Specifies datatype or texttype maps from GDSII or OASIS input to Calibre layer numbers. 
    - `13` is the source layer
    - `DATATYPE` is used to map drawn gerometric layers.
    - `0` is the source type that specifies particular datatype.
    - `5013` is the target layer which specifies a layer number to be used by calibre.
3. `layer MAP 13 TEXTTYPE 3 50133`
    - `TEXTTYPE` is used to map text layer objects.  
    
- **TEXT Layer**: Specifies the layers in the database from which connectivity extraction text is read.  
Syntax: `TEXT LAYER <layer>`

- **PORT LAYER TEXT**: For input layout databases, causes text objects on the specified layer(s) in the top-level cell to be read and treated as text ports.  
Syntax: `PORT LAYER TEXT <layer>`

- **ATTACH**: Transfers connectivity information from one layer to another. Used primarily for text label attachment.  
Syntax: `ATTACH layer1 layer2`
   - `layer1`: A required original layer.
   - `layer2`: A requrired original layer/layout set or derived polygon layer. This layer must appear as an input layer to CONNECT or SCONNECT operations.  
   - Example: `ATTACH POLY1_TEXT p1trm`
     - `POLY1_TEXT` : It is poly1 text layer.
     - `p1trm` : It is derived layer used in Resistor rule file example. 
     
- **LVS BLACK BOX PORT**: Defines port objects for the LVS Box BLACK statement.
Syntax: `LVS BLACK BOX PORT <original_layer> <text_layer> <interconnect_layer>`
   - `original _layer` is a required layer/layer set that forms a port for a black box cell.  
   - `text_layer` is used for naming of ports. Text layer objects must be at the same hierarchical level as the original layer.
   - `interconnect_layer` is a required layer/layer set or a derived layer containing objects that connects to the original layer.
   - Example: `LVS BLACK BOX PORT poly_dg POLY1_TEXT p1trm`

- **RECTANGLES**: Generates an output layer consisting of an array of rectangles with the specified dimensions and spacing.  
Syntax: `RECTANGLES <width length> <spacing> <offset> <INSIDE OF LAYER layer> <MAINTAIN SPACING>`  
   - `width length` represents x and y axis of a rectangle.
   - `spacing` represents spacing between rectangles in x and y axis.
   - `offset` is an optional keyword that specifies the horizontal and vertical offsets between adjacent rectangles.
   - `INSIDE OF LAYER layer` Specifies a layer having polygoins to be filled with rectangles.
   - `MAINTAIN SPACING` is an optinal keyword that controls the spacing of rectangles.
   - Example: `thinmet_fill_all = RECTANGLES 5 2 2 INSIDE OF LAYER (EXTENT)`
   
- **CONNECT**: Defines electrical connections on input layers.  
Syntax: `CONNECT <layer1> <layer2>......<layer N> BY <layer C>`  
   - `<layer1> <layer2>......<layer N>` are required original layers/layer sets or a derived polygon layers.
   - `<layer C>` specifies a contact, cut or via layer.
   - Example: `CONNECT p1trm m1trm BY CONT`
   
- **DMACRO**: A MACRO definition is known as DMACRO. MACROS are used to make a sequence of computing instructions available to the programmer as a single program statement.  
Syntax:   `
```bash
DMACRO macro_name[arguments]
         {
         SVRF Code
         }
```
   - Example:  
```bash  
DMACRO getWLRes seed {[
property l, w
weff = 0.5
ar   = area(seed)
w    = 0.5 * (perimeter_coincide(pos,seed) + (perimeter_coincide(neg,seed)))
l    = ar/w
        if (bends(seed) > 0)
        {
        if  (W > L)
        w = w - weff*bends(seed) * l
        else
        l = l - weff*bends(seed) * w
        }
]}
```
   - `getWLRes` is the macro name which is used in the poly-resistor rule file.
   - `seed` is the argument of the macro.  
  
- **CMACRO**: It is a keyword to invoke a macro.  
Syntax: `CMACRO macro_name [arguments]`
   - `macro_name` must match its coresponding DMACRO definition.
   - `arguments` may be either a name of layer or numeric constant.
   
## **Writing MACRO Statements**

This section shows the different ways to write MACRO statements. It includes macro statements to calculate W & L of a Resistor. Some inbuilt functions such as bends(), perimeter(), area(), perimeter_coincide() are used here.  

- `bends`: Returns the total bends in the shapes of the specified pin or layer. The result is expressed in units of right angles.Bends value can be calculated by summing the angle in degrees, by which the perimeter changes direction at all concave vertices and dividing by 90 to convert to units of right angle bends.    
Syntax: `BENDS(pin_or_layer)`  
![Bends](https://github.com/vsao/verification/blob/main/Rule_deck/bends.png)

- `perimeter` : Returns the total length of the perimeter of the shapes for the specified pin or layer.  
Syntax: `PERIMETER(pin_or_layer)`    

- `area` : Returns the total area of shapes that are part of the specified pin or layer.  
Syntax: `AREA(pin_or_layer)`  

- `perimeter_coincide` : Returns the total length of the parts of perimeters on the first pin or layer that coincide with the perimeter of the second pin or layer.  
Syntax: `PERIMETER_COINSIDE(pin_or_layer, pin_or_layer)`  

1. Macro Staement for a rectangular Poly-Resistor   
```bash  
DMACRO getWLRes seed {[
property l, w
pr   = perimeter(seed)
w    = 0.5 * (perimeter_coincide(pos,seed) + (perimeter_coincide(neg,seed)))
l    = (pr-2w)/2
]}
```  
2. Macro Statment for Serpentaine 90 Poly-Resistor    
2.1 Without Bends  
```bash  
DMACRO getWLRes seed {[
property l, w
pr   = perimeter(seed)
w    = 0.5 * (perimeter_coincide(pos,seed) + (perimeter_coincide(neg,seed)))
l    = (pr-2w)/2
]}
```  
2.2 With Bends  
```bash  
DMACRO getWLRes seed {[
property l, w
n    = bends(seed)
pr   = perimeter(seed)
w    = 0.5 * (perimeter_coincide(pos,seed) + (perimeter_coincide(neg,seed)))
l    = (0.5*pr)-(1+(0.5*n))*w
]}  
```  
2.3 Comparison betwwen Mcaro statements: with bends and without bends  

MACRO statements without bends does not include effect of bends on width and lenght of a Resistor hence l calculation using without bends statements gives some error. For the better picture, error comparison between with bends and without bends statements for a short strip length and a long strip length poly-resistor is carried out.  
2.3.1 Short Strip length (Serpentine 90 Poly-Resistor with 2 strips)   

| Parameters | Without Bends(u) | With Bends(u) | Error(%) |
|------------|------------------|---------------|----------|
|     l      | 24.52            | 22.52         | 8.88     |
|     w      | 2                | 2             | 0        |

2.3.2 Long Strip length (Serpentine 90 Poly-Resistor with 2 strips)

| Parameters | Without Bends(u) | With Bends(u) | Error(%) |
|------------|------------------|---------------|----------|
|     l      | 186.39           | 184.39        | 1.08     |
|     w      | 2                | 2             | 0        |

    
 Here, we can conclude that error decreases by increasing the strip length of a resistor.
 
 ## Number of bends calculation
- Custom Poly-Resistor(rnp1)
  - Layout shape is shown below:  
  
  ![Custom Poly-Resistor](https://github.com/vsao/verification/blob/main/Rule_deck/poly_res.png)
  
  - Number of bends calculation: Area Macro is used to calculate w and l of resistor for both the cases: `with bends` and `without beds`.

| Parameters | Without Bends(u) | With Bends(u) |
|------------|------------------|---------------|
|     l      | 4.75878          | 4.75878       |
|     w      | 6.0825           | 2.51341       |

```bash

l(with bends) = l(without bends) - (0.5*(bends)*w)
2.51341       = 6.0825 - 0.5*bends*4.75878
bends         = 1.5
```
- Serpentine 45 Poly-resistor: To verify the bends calculation method sepentine 45 resistor is taken into account.  
  - Layout shape is shown below:  
  
  ![Serpentine 45 Poly-Resistor](https://github.com/vsao/verification/blob/main/Rule_deck/res45.png)
  
  - Number of bends calculation: Area Macro is used to calculate w and l of resistor for both the cases: `with bends` and `without beds`.

| Parameters | Without Bends(u) | With Bends(u) |
|------------|------------------|---------------|
|     l      | 23.9774          | 21.9774       |
|     w      | 2                | 2             |

```bash

l(with bends) = l(without bends) - (0.5*(bends)*w)
21.9774       = 23.9774 - 0.5*bends*2
bends         = 2
```

 












