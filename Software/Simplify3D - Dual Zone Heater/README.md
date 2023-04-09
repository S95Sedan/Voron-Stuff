Just wanted to add this incase someone else is setting up something similair.

My own code is based on this page;<br>
https://klipper.discourse.group/t/automatic-control-of-rectangular-heatbed-sections-based-on-print-area/3252

This code mainly started as i always wanted a dual zone heater setup, for more flexibility.<br>
Shown below is a picture of my current bed setup;
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Software/Simplify3D%20-%20Dual%20Zone%20Heater/images/image_01.jpg | width=300)

Simplify3D - Start script:
```
M104 S0 ; Stops sending temp waits separately
M140 S0
PRINT_START EXTRUDER=[extruder0_temperature] BED_TEMP=[bed0_temperature] AREA_START=[build_min_x],[build_min_y] AREA_END=[build_max_x],[build_max_y]
```

Simplify3D - End script:
```
PRINT_END
```

Klipper - Start Script:<br>
(Based on; https://github.com/jontek2/A-better-print_start-macro )
```
[gcode_macro PRINT_START]
gcode:
  # This part fetches data from your slicer. Such as bed temp, extruder temp, chamber temp and size of your printer.
  {% set target_extruder = params.EXTRUDER|int %}
  {% set target_chamber = params.CHAMBER|default("40")|int %}
  {% set x_wait = printer.toolhead.axis_maximum.x|float / 2 %}
  {% set y_wait = printer.toolhead.axis_maximum.y|float / 2 %}

#Set the coordinates of the heatbed sections
  {% set x0 = 0 %}       # Minimum bed size - X Axis
  {% set x1 = 197 %}     # Halfway
  {% set x2 = 395 %}     # Maximum bed size - X Axis

  {% set y0 = 0 %}       # Minimum bed size - Y Axis
  {% set y1 = 350 %}     # Maximum bed size - Y Axis

#Get parameters from Slicer
  {% set BED_TEMP = params.BED_TEMP|default(50)|float %}       # This gets the BED_TEMP from the slicer)
  {% if params.AREA_START and params.AREA_END %}               # This gets the AREA_START and AREA_END from the slicer)
      {% set A = params.AREA_START.split(",")[0]|float %}
      {% set B = params.AREA_START.split(",")[1]|float %}

      {% set C = params.AREA_END.split(",")[0]|float %}
      {% set D = params.AREA_END.split(",")[1]|float %}
  {% endif %}

# Homes the printer, sets absolute positioning and updates the Stealthburner leds.
  G28                   # Full home (XYZ)
  G90                   # Absolute position

  BED_MESH_CLEAR        # Clears old saved bed mesh (if any)

   # Checks if the bed temp is higher than 90c - if so then trigger a heatsoak.
    {% if params.BED_TEMP | int > 90 %}
      M106 S255                                                                      # Turns on the PT-fan
      G1 X{x_wait} Y{y_wait} Z15 F9000                                               # Goes to center of the bed
      #Check and enable the required heatbeds
          #Heatbed Left  x0,y0  x1,y1
            {% if ((C > x0) and (D > y0) and (A < x1) and (B < y1)) %}               # Checks if theres something on the Left Side
                {% set BED_LEFT = params.BED_TEMP | default(0) | float %}            # Sets the Left temperature to the slicer BED_TEMP
                SET_HEATER_TEMPERATURE HEATER=heater_bed_left TARGET={BED_LEFT}      # Enables the Left Bed
            {% endif %}
          #Heatbed Right  x1,y0  x2,y1
            {% if ((C > x1) and (D > y0) and (A < x2) and (B < y1)) %}               # Checks if theres something on the Right Side
                {% set BED_RIGHT = params.BED_TEMP | default(0) | float %}           # Sets the Right temperature to the slicer BED_TEMP
                SET_HEATER_TEMPERATURE HEATER=heater_bed_right TARGET={BED_RIGHT}    # Enables the Right Bed
            {% endif %}

      SET_DISPLAY_TEXT MSG="L: {BED_LEFT}c R: {BED_RIGHT}c"                           # Display Heater info. (Debugging)
      G4 P5000                                                                        # Waits 5 seconds to display values.
      SET_DISPLAY_TEXT MSG="Heating chamber to: {target_chamber}c"                    # Displays info for heating the chamber
      TEMPERATURE_WAIT SENSOR="temperature_sensor chamber" MINIMUM={target_chamber}   # Waits for chamber to reach desired temp

  # If the bed temp is not over 90c, then it skips the heatsoak and just heats up to set temp with a 5min soak
    {% else %}
      # SET_DISPLAY_TEXT MSG="Heater below 90c Loading"                                # Display Heater info. (Debugging)
      # G4 P10000                                                                      # Waits 10 seconds to display values.
      #Check and enable the required heatbeds
          #Heatbed Left  x0,y0  x1,y1
            {% if ((C > x0) and (D > y0) and (A < x1) and (B < y1)) %}
                {% set BED_LEFT = params.BED_TEMP | default(0) | float %}
                SET_HEATER_TEMPERATURE HEATER=heater_bed_left TARGET={BED_LEFT}        # Enables the Left Bed
            {% endif %}
          #Heatbed Right  x1,y0  x2,y1
            {% if ((C > x1) and (D > y0) and (A < x2) and (B < y1)) %}
                {% set BED_RIGHT = params.BED_TEMP | default(0) | float %}
                SET_HEATER_TEMPERATURE HEATER=heater_bed_right TARGET={BED_RIGHT}      # Enables the Right Bed
            {% endif %}
      SET_DISPLAY_TEXT MSG="Waiting 2 minutes to stabilize."                           # Waits 2 min for the bedtemp to stabilize
      G4 P120000                                      
    {% endif %}

  ##  Uncomment for V2 (Quad gantry level AKA QGL)
  SET_DISPLAY_TEXT MSG="Leveling Z"      # Displays info
  #STATUS_LEVELING                       # Sets SB-leds to leveling-mode
  quad_gantry_level                      # Levels the buildplate via QGL
  G28 Z                                  # Homes Z again after QGL

  ##  Uncomment for bed mesh (2 of 2)
  SET_DISPLAY_TEXT MSG="Loading Bed Mesh"     # Displays info
  #STATUS_MESHING                             # Sets SB-leds to bed mesh-mode
  #bed_mesh_calibrate                         # Starts bed mesh (Old)
  BED_MESH_PROFILE LOAD=Heated_100c

  # Heats up the nozzle up to target via data from slicer
  SET_DISPLAY_TEXT MSG="Hotend: {target_extruder}c"             # Displays info
  #STATUS_HEATING                                               # Sets SB-leds to heating-mode
  G1 X{x_wait} Y{y_wait} Z15 F9000                              # Goes to center of the bed
  M107                                                          # Turns off partcooling fan
  M109 S{target_extruder}                                       # Heats the nozzle to printing temp

  # Cleans the nozzle before starting
  CLEAN_NOZZLE

  # Gets ready to print by doing a purge line and updating the SB-leds
  SET_DISPLAY_TEXT MSG="Starting print..."         # Displays info
  STATUS_PRINTING                                  # Sets SB-leds to printing-mode
  G0 X{x_wait - 50} Y4 F10000                      # Moves to starting point
  G0 Z0.4                                          # Raises Z to 0.4
  G91                                              # Incremental positioning 
  G1 X100 E20 F1000                                # Purge line
  G90                                              # Absolut position     
```

Klipper - End Script: 
```
[gcode_macro PRINT_END]
gcode:
    M400                           ; wait for buffer to clear
    G92 E0                         ; zero the extruder
    G1 E-5 F1800                   ; retract filament

      # Cleans the nozzle after printing
      CLEAN_NOZZLE

    G91                            ; relative positioning
    G0 Z1.00 X20.0 Y20.0 F20000    ; move nozzle to remove stringing
      TURN_OFF_HEATERS
    M107                           ; turn off fan
    G1 Z2 F3000                    ; move nozzle up 2mm
    G90                            ; absolute positioning
    G0  X125 Y250 F3600            ; park nozzle at rear
      BED_MESH_CLEAR
```

