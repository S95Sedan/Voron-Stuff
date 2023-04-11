- Edit 11-04-2023: Cleaned up the code alot, instead of everything being inside PRINT_START, its now a seperate HEATBED_ZONES macro.
- Edit 10-04-2023: Changed the split() parameters from , to - as Simplify3D doesnt save it otherwise.<br>

Just wanted to add this incase someone else is setting up something similair.<br>

My own code is based on this page;<br>
https://klipper.discourse.group/t/automatic-control-of-rectangular-heatbed-sections-based-on-print-area/3252

This code mainly started as i always wanted a dual zone heater setup, for more flexibility.<br>
Shown below is a picture of my current bed setup;
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Software/Dual%20Zone%20Heater/images/image_01.jpg)

Simplify3D - Start script:
```
M104 S0 ; Stops sending temp waits separately
M140 S0
PRINT_START EXTRUDER=[extruder0_temperature] BED_TEMP=[bed0_temperature] AREA_START=[build_min_x]-[build_min_y] AREA_END=[build_max_x]-[build_max_y]
```

Simplify3D - End script:
```
PRINT_END
```

Flipper - Heatbed Zones;
```
[gcode_macro HEATBED_ZONES]
gcode:
	#Set the coordinates of the heatbed sections
		{% set x0 = 0 %}		# X-Axis - Minimum coordinate.
		{% set x1 = 197 %}		# X-Axis - Center
		{% set x2 = 395 %}		# X-Axis - Maximum coordinate.

		{% set y0 = 0 %}		# Y-Axis - Minimum coordinate.
		{% set y1 = 350 %}		# Y-Axis - Maximum coordinate.

	#Get parameters from Slicer
		{% set BED_TEMP = params.BED_TEMP|default(0)|float %}			# Gets the bed temparature sent by the slicer.

	# SET_DISPLAY_TEXT MSG="Heatbed - Input Temperature:{BED_TEMP}"			# Bed Temperature info. (For Debugging)
	# G4 P10000									# Waits 10 seconds to display values.

	{% if params.AREA_START and params.AREA_END %}
		{% set A = params.AREA_START.split("-")[0]|float %}
		{% set B = params.AREA_START.split("-")[1]|float %}

		{% set C = params.AREA_END.split("-")[0]|float %}
		{% set D = params.AREA_END.split("-")[1]|float %}

	# SET_DISPLAY_TEXT MSG="Heatbed - AREA_MIN:{A}+{B} AREA_MIN:{C}+{D}"		# Area_min and Area_max info. (For Debugging)
	# G4 P10000									# Waits 10 seconds to display values.

	#Check and enable the required heatbeds
		#Heatbed Left  x0,y0  x1,y1
		{% if ((C > x0) and (D > y0) and (A < x1) and (B < y1)) %}				# Compares the coordinates for the left bed.
			{% set temp_bed_left = params.BED_TEMP|default(0)|int %}
			SET_HEATER_TEMPERATURE HEATER=heater_bed_left TARGET={temp_bed_left}		# Sets the target temp for the left bed.
		{% endif %}
		#Heatbed Right  x1,y0  x2,y1
		{% if ((C > x1) and (D > y0) and (A < x2) and (B < y1)) %}				# Compares the coordinates for the right bed.
			{% set temp_bed_right = params.BED_TEMP|default(0)|int %}
			SET_HEATER_TEMPERATURE HEATER=heater_bed_right TARGET={temp_bed_right}		# Sets the target temp for the right bed.
		{% endif %}
		
		# SET_DISPLAY_TEXT MSG="Heatbed - Left{temp_bed_left} Temp Right{temp_bed_right}"	# Left/Right Temperature info. (For Debugging)
	    # G4 P10000											# Waits 10 seconds to display values.
		
	{% endif %}
```

Klipper - PRINT_START add:<br>
```
  # Checking which heatbed zones to heat.
  HEATBED_ZONES BED_TEMP={params.BED_TEMP|default(0)|float} AREA_START={params.AREA_START|default("0-0")} AREA_END={params.AREA_END|default("0-0")}
```

