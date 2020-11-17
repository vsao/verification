# Verification
drc/lvs rule check  

# Steps to do Schematic and Layout in tanner    
- Type "sedstart" to open shematic viewer.         
- In schematic editor go to `file->new->New Library` and craete a new library.  
eg: my_lib  
- Go to `cell->new view` and create new cell view.  
eg: my_resister  
- Open cell view(my_resister) and place pins to both the terminals with the names t1 and t2.
- Save the Schematic View.  
- Type "ledstart" to open layout viewer.  
- Go to `setup->Technology reference` and set the appropriate technology.  
- In layout editor go to "my_lib" library.  
- Go to `cell->new view` and create new cell view with the same name as schematic view.  
- In Schematic Viewer click on SDL.  
- Go to layout editor and check for the layout view of cell.  
- After creating layout instances, Place the pins in appropriate terminals.  
- To change the pin layer to "Metal1 text", Select the pin and click on edit objects then select the layer as "MET1 text".  
- Select the layout and go to `Draw->Guard Rings->Draw Guard Ring around selection` and draw a PDC Guard Ring.  
- Save the layout View.

# DRC/LVS (Calibre on Local Linux Host)  
**NOTE** The following instructions are for running Calibre on the Linux host. To run verification on Windows, a SSH tunnel to the Linux host has to be created using PuTTy and a network drive has to be setup to share between the Windows and Linux host.
