# Gerbers for the latest verion added, any suggestions and/or problems feel free to contact me on the voron discord.

Already had the idea for a while but design mainly came together due to there not being much options out there for breakout pcb's and to include some features which i personally might use.

It should be 100% compatible with all the outputs found on Hartk's Stealthburner PCB.
Which can be found here; https://github.com/hartk1213/MISC/blob/main/PCBs/Stealthburner_Toolhead_PCB/

Ontop of that it has added connections for both the X and Y endstops, including 5v for Hall effect sensors.<br>
And an output to connect Electronic fans.<br>
Aswell as having a 14+2pin molex micro-fit cable thats the same on both sides.

Breakout PCB - Front:<br>
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Breakout%20PCB/images/BreakoutPCB_01.jpg)

Breakout PCB - Back:<br>
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Breakout%20PCB/images/BreakoutPCB_02.jpg)


# BOM
| Brand | Pins | Part Number | Qty |
| :------------: | :-:| :----:|------------: 
| Molex | 2 | 436500200 | x2 |
| Molex | 2 | 436450200 | x2 |
| Molex | 2 | 430450200 | x1 |
| Molex | 2 | 430250200 | x1 |
| Molex | 14 | 430451400 | x1 |
| Molex | 14 | 430251400 | x1 |
| Molex | 4 | 430450400 | x1 |
| Molex | 4 | 430250400 | x1 |
| JST-XH | 2 | n/a | x7 |
| JST-XH | 3 | n/a | x1 |
| JST-XH | 1 | n/a | x2 |

# Breakout PCB - Control Board Side (input)<br>
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Breakout%20PCB/images/BreakoutPCB_03.jpg)

## Top Row:
| PCB | | Wire AWG |
|:-:|:-:|:-:|
|TH0  | Hotend Thermistor Signal Pin (TH0) | 24 |
|AGND | Hotend Thermistor -V | 24 |
|HEF  | Hotend Cooling Fan -V | 24 |
|PCF  | Part Cooling Fan -V | 24 |
|24V  | HE0 +V | 20   | 24v |
|GND  | PSU -V (NOT MAINS GND) | 20 |
|HE0  | Hotend Heater -V | 20 |
|5V   | 5V | 20   |
|FS   | ERCF Filament sensor | 24 |
|ABL | Probe/Klicky Signal Pin | 24 |

## Middle Row:
| PCB | | Wire AWG | Type |
|:-:|:-:|:-:|:-:|
|AGND | Hotend Thermistor -V | 24 |
|AUX  | Auxillary |  24  |
|GND  | PSU -V (NOT MAINS GND) | 24 |
|ECF   | Electronics Fan |24  |
|LED  | Neopixel Data Pin |24  |
|X   | X endstop |24  |
|GND  | PSU -V (NOT MAINS GND) | 24   |
|X   | X endstop |24  |
|GND  | PSU -V (NOT MAINS GND) | 24   |

## Bottom Row:
| PCB | | Wire AWG | Type |
|:-:|:-:|:-:|:-:|
|S2B  | Black Stepper Wire |24  |
|S2A  | Green Stepper Wire |24  |
|S1A  | Red Stepper Wire |24 |
|S1B  | Blue Stepper Wire |24  |

# Breakout PCB - Extruder Side (output)<br>
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Breakout%20PCB/images/BreakoutPCB_04.jpg)

Extruder Side Wiring:
| PCB | 14-Pin Molex | Wire AWG |
|:-:|:-:|:-:|
|PROBE| Probe/Klicky Signal Pin | 24   |
|24V  | HE0 +V | 20   |
|PCF  | Part Cooling Fan -V | 24  |
|AGND | Hotend Thermistor -V |24   |
|TH0  | Hotend Thermistor Signal Pin (TH0) | 24   |
|GND  | PSU -V (NOT MAINS GND) | 24   |
|AUX  | Auxillary |  24  |
| | | |
|HE0  | Hotend Heater -V | 20 |
|5V   | 5V | 24   |
|HEF  | Hotend Cooling Fan -V |  24|
|S1A  | Red Stepper Wire |24 |
|S2A  | Green Stepper Wire |24  |
|S1B  | Blue Stepper Wire |24  |
|S2B  | Black Stepper Wire |24  |

| PCB | 2-Pin Molex | Wire AWG |
|:-:|:-:|:-:|
|LED  | Neopixel Data Pin |24  |
|FS   | ERCF Filament sensor |24  |

| PCB | 4-Pin Molex | Wire AWG |
|:-:|:-:|:-:|
|GND  | PSU -V (NOT MAINS GND) | 24   |
|Y  | Y endstop |24  |
|X   | X endstop |24  |
|5V   | 5V | 24   |

| PCB | 2-Pin | Wire AWG |
|:-:|:-:|:-:|
|ECF   | Electronics Fan |24  |
|5V   | 5V | 24   |



