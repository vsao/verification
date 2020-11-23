# Verification
drc/lvs rule check  

# Steps to do Schematic and Layout in tanner    
- Type "sedstart" to open shematic viewer.         
- In schematic editor go to `file->new->New Library` and create a new library.  
eg: my_lib  
- Go to `cell->new view` and create new cell view.  
eg: my_register  
- Open cell view(my_register) and place pins to both the terminals with the names t1 and t2.
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
**NOTE:** The following instructions are for running Calibre on the Linux host. To run verification on Windows, a SSH tunnel to the Linux host has to be created using PuTTy and a network drive has to be setup to share between the Windows and Linux host.

# DRC
- Setup the server and path to Calibre. Click the **Settings** button (Spanner icon) next to Calibre Toolbar (DRC/LVS/PEX/RVE/Settings):
  - For the host enter: `192.168.6.50`
  - For calibre give the entire path to the current tool:
    - `$CALIBRE_HOME/bin/calibre`
- In L-Edit, click **Run Calibre DRC** from the Calibre toolbar which will launch the GUI.
- If the `Load Runset File` dialog box appears, you can **Cancel** it.
- In the GUI, click the **Rules** button in the left Panel and enter the following info:
  - `DRC Rules File:` Browse to the Calibre rule file eg.
    - `$XFAB_CALIBRE_RUNSET/xt018_1243` You can click Load to test any problem with the rule file.
  - Check Selection Recipe: Ignore
  - `DRC Run Directory`: Ideally `$PROJDIR/DRC`
- Click `Inputs` and setup the inputs but the defaults should be Ok eg. GDSII/Export from Layout Viewer
- Click `Outputs` and the defaults should be fine.
- If none of the left panel buttons are red, click **Run DRC**
- If the run was successful, save the GUI settings to a file by clicking `File->Save` Runset which can be loaded the next time.

# LVS
- In L-Edit, click **Run Calibre LVS** from the Calibre toolbar which will launch the GUI.
- If the `Load Runset File` dialog box appears, you can **Cancel** it.
- In the GUI, click the **Rules** button in the left Panel and enter the following info:
  - `LVS Rules File`: Browse to the Calibre rule file eg.
    - `$XFAB_CALIBRE_RUNSET/xt018_1243`
  - `LVS Run Directory`: Ideally `$PROJDIR/DRC`
- Click **Inputs** and set up the **Run**, **Step**, and **Layout** tab information. Settings may be correct by default:
  - Select **Layout vs Netlist** from the Step dropdown menu.
  - Enable **Export from layout viewer** on the **Layout** tab.
  - Select **GDSII** in the Format dropdown list.
- Click the **Netlist** tab on the Inputs pane to set up schematic (source) input.
- Enable **Export from schematic viewer**. **NOTE:** If you want to LVS with netlist from another cell, open that cell in S-Edit and it will netlist that cell. If a netlist exists and you to LVS wrt that netlist then deselect that this button.
- Select **SPICE** from the Format dropdown list.
- Click **Outputs** and enable **View Extraction Report** after LVS finishes and Start RVE after LVS finishes.
