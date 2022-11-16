-- Currently still unreleased until i recieve my sample pcb's to review everything --

Mainly designed this since i wasn't too happy with the other PCB solutions out there.
Features all the inputs found on Hartk's Afterburner/Stealthburner PCB. (Found here; https://github.com/hartk1213/MISC/blob/main/PCBs/Stealthburner_Toolhead_PCB/ )

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
| JST-XH | 2 | n/a | 7 |
| JST-XH | 3 | n/a | 3 |
| JST-XH | 4 | n/a | 2 |

# Breakout PCB - Control Board Side (input)<br>
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Breakout%20PCB/images/BreakoutPCB_03.jpg)

## Middle Row  (Left to Right):
| PCB | | Wire AWG | Type |
|:-:|:-:|:-:|:-:|
|TH0  | Hotend Thermistor Signal Pin (TH0) | 24   |
|AGND | Hotend Thermistor -V |24   |
|HEF  | Hotend Cooling Fan -V |  24|
|24V  | HE0 +V | 24   | Optional |
|24V  | HE0 +V | 20   | 24v for Everything |
|HE0  | Hotend Heater -V | 20 | Needed for Hotend |
|5V   | 5V | 20   | 5v for Everything |
|GND  | PSU -V (NOT MAINS GND) | 20 | GND for Everything |
|24V  | HE0 +V | 24   | Optional |
|GND  | PSU -V (NOT MAINS GND) | 24 | Optional |
|FS   | ERCF Filament sensor |24  |


## Middle Row:
| PCB | | Wire AWG | Type |
|:-:|:-:|:-:|:-:|
|AGND | Hotend Thermistor -V |24   |
|AUX  | Auxillary |  24  |
|GND  | PSU -V (NOT MAINS GND) | 24   | Optional |
|PCF  | Part Cooling Fan -V | 24  |
|24V  | HE0 +V | 24   | Optional |
|24V  | HE0 +V | 24   | Optional |
|GND  | PSU -V (NOT MAINS GND) | 24   | Optional |
|ABL | Probe/Klicky Signal Pin | 24   |

## Bottom Row:
| PCB | | Wire AWG | Type |
|:-:|:-:|:-:|:-:|
|S2B  | Black Stepper Wire |24  |
|S2A  | Green Stepper Wire |24  |
|S1A  | Red Stepper Wire |24 |
|S1B  | Blue Stepper Wire |24  |
|ECF   | Electronics Fan |24  |
|LED  | Neopixel Data Pin |24  |
|X   | X endstop |24  |
|GND  | PSU -V (NOT MAINS GND) | 24   | Optional |
|X   | X endstop |24  |
|GND  | PSU -V (NOT MAINS GND) | 24   | Optional |

# Breakout PCB - Extruder Side (output)<br>
![](https://github.com/S95Sedan/Voron-Stuff/blob/main/Breakout%20PCB/images/BreakoutPCB_04.jpg)

Extruder Side Wiring:
| PCB | 14-Pin Molex | Wire AWG |
|:-:|:-:|:-:|
|24V  | HE0 +V | 20   |
|GND  | PSU -V (NOT MAINS GND) | 24   |
|PROBE| Probe/Klicky Signal Pin | 24   |
|HE0  | Hotend Heater -V | 20 |
|5V   | 5V | 24   |
|PCF  | Part Cooling Fan -V | 24  |
|HEF  | Hotend Cooling Fan -V |  24|
|AGND | Hotend Thermistor -V |24   |
|TH0  | Hotend Thermistor Signal Pin (TH0) | 24   |
|AUX  | Auxillary |  24  |
|S1A  | Red Stepper Wire |24 |
|S2A  | Green Stepper Wire |24  |
|S1B  | Blue Stepper Wire |24  |
|S2B  | Black Stepper Wire |24  |

| PCB | 2-Pin Molex | Wire AWG |
|:-:|:-:|:-:|
|LED  | Neopixel Data Pin |24  |
|FS   | ERCF Filament sensor |24  |

| PCB | 4-Pin | Wire AWG |
|:-:|:-:|:-:|
|GND  | PSU -V (NOT MAINS GND) | 24   |
|X   | X endstop |24  |
|Y  | Y endstop |24  |
|5V   | 5V | 24   |

| PCB | 2-Pin | Wire AWG |
|:-:|:-:|:-:|
|GND  | PSU -V (NOT MAINS GND) | 24   |
|ECF   | Electronics Fan |24  |


