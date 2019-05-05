<div align="center"><h1 style="font-family: courier;" align="center">satstep2660</h1></div>

<div align="center"><img src="media/front1.jpg" width="100%"></div>
<div align="center"><i>An open source & fabbable stepper driver board based on TMC2660.</i></div>

The board
--
Born as an evolution of **[satstep6600](https://github.com/satstep/satstep6600)**, **satstep2660** is an stepper driver board that aims to be an advanced combination of features/performance while keeping a **FabLab-reproducible open source design**. Apart from just improving some of the satstep6600 features, such as microstepping precision, it also implements **additional programmability and sensing functionalities**. The **[TMC2660](https://www.trinamic.com/products/integrated-circuits/details/tmc2660-pa/)** on which satstep2660 is based, offers an **SPI interface**, allowing to dynamically change several of the configuration parameters, for instance microstepping and current among others. The **[Trinamic](https://www.trinamic.com/)** chip is also able to **measure the back EMF** of the motor, detecting  potential stall situation and automatically adjusting the current according to the detected load. If correctly tuned, the stall detection can be used in place of endstops and even to detect missed steps.
 
**satstep2660/TMC2660 features**:

- up to **256 microstepping** 
- motor current up to **4A per coil**
- voltage up to **30V** DC 
- **overcurrent, short to GND, overtemperature and undervoltage** protections
- **SPI** and **STEP/DIR** interfaces
- **on the fly reconfigurable: microstepping, current, stall sensitivity**...
- back **EMF sensing**, **stall detection** (no need of endstops!), automatic **load sensing**, **load-dependent current control**
- capable of **detecting missed steps**
- selectable clock, internal **15Mhz** or external **16Mhz (or your preferred value 10-20Mhz)**
- symmetric sense resistor pcb design
- massive polygons and double-side pcb to **maximize the heat dissipation**
- exposed **5V regulated output**
- cost of about **11$** 

<img src="media/noheat2.jpg" width="100%">

Both external clock and sense resistors can be changed according to specific needs. In the given schematic the external clock, set to 16Mhz to match with my other **[satshakit boards](https://github.com/satshakit/)** running at the same frequency, can host an oscillator with the same footprint having different clock. Same is for the 1206 sense resistor, set to 0.1Ω in this configuration.

**satstep2660 schematic**:

<img src="media/satstep2660_schematic.png" width="100%">

The PCB takes care of the **[TMC2660 recommendations](https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC26x_Getting_started.pdf)** and **[TMC2660 Datasheet](https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC2660_datasheet.pdf)**, like the **symmetric design** for sense resistor and motor channel, **massive layers** for motor channels GND and VMOT, optional **external clock** for precise operations. A **dual-layer PCB** design has been used, due to Fab Lab/milling limitations on producing the recommended 4 layer design. The top layer outer polygon is the VMOT.

**satstep2660 board top**:

<img src="media/satstep2660_board.png" width="100%">

To be noted that an almost equal design of the motor channel polygons is in the bottom layer, to leave as much as surface as possible for **heat dissipation**. In this case the outer layers are GND, with the **logic GND** on left side connected to the **power GND** on the right, with a small trace to reduce noise between the two.

**satstep2660 board bottom**:

<img src="media/satstep2660_bottom.png" width="100%">

As shown in the following gif, the PCB has been made from a **double-sided FR1 copper clad**, instead of two glued as in the satstep6600. Despite a bit longer and precise process (described below), this allows trough hole components to be correctly soldered on both sides.

<img src="media/satstep2660.gif" width="100%">

Downloads
--

**Downloads (right click download as):**

- **[satstep2660 schematic](https://github.com/satstep/satstep2660/raw/master/eagle/satstep2660.sch)**
- **[satstep2660 board](https://github.com/satstep/satstep2660/raw/master/eagle/satstep2660.brd)**
- **[satstep2660 top traces (1/64)](https://raw.githubusercontent.com/satstep/satstep2660/master/milling/top_traces.png)**
- **[satstep2660 top traces (0.01")](https://raw.githubusercontent.com/satstep/satstep2660/master/milling/top_traces_001.png)**
- **[satstep2660 top holes](https://raw.githubusercontent.com/satstep/satstep2660/master/milling/top_holes.png)**
- **[satstep2660 top reference holes](https://raw.githubusercontent.com/satstep/satstep2660/master/milling/top_reference_holes.png)**
- **[satstep2660 bottom traces](https://raw.githubusercontent.com/satstep/satstep2660/master/milling/bottom_mirrored.png)**
- **[satstep2660 BOM xlsx](https://github.com/satstep/satstep2660/raw/master/bom/satstep2660%20BOM.xlsx)**
- **[satstep2660 BOM ods](https://github.com/satstep/satstep2660/raw/master/bom/satstep2660%20BOM.ods)**

Getting started with satstep2660
--

As usual for CNC milled and hand soldered boards, before connecting anything make sure all the traces and components are properly isolated. Especially in this case where there is up to 30V and spikes can occur, **carefully check all the board for shorts**. 

Another mandatory step is to make sure you **have set the clock configuration jumpers** depending if you will use the internal or the external clock. The internal clock is set to 15Mhz and the external to 16Mhz (or as per the oscillator you choose). External clock is recommended for more precise operations, and to possibly to match the frequency of the used microcontroller. Following how to configure the **satstep2660 clock**: 

- **internal clock**: "OSC JUMPER" connected to the left to close the "INT. OSC"
- **external clock**: "OSC JUMPER" connected to the right to close "EXT 16M", and "EXT OSC EN JUMPER" in place

Find below the **satstep2660 pinout**:

<img src="media/satstep2660_pinout.png" width="100%">

In case you would like to use an **Arduino to drive satstep2660**, here an example connection schema for SPI using pin 10 as slave select. To test satstep2660 is recommended to use the **[Trinamic TMC26XX library](https://github.com/trinamic/TMC26XStepper)**, example **[TMC26XXMotorTest](https://github.com/trinamic/TMC26XStepper/tree/master/examples/TMC26XMotorTester)** connecting both the SPI and the STEP/DIR interfaces. Make sure to assign the  pins according to your connections to the slave select and the STEP/DIR interface. The TMC2660 is configured by using 5 different registers with 20bit each, for more information about the values refer to the  **[TMC2660 Datasheet](https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC2660_datasheet.pdf)**.

<img src="media/satstep2660_arduino_connection.png" width="100%">

Milling tips
--

To mill the board both **1/64 and 0.01" tools** together with **[fabmodules](http://fabmodules.org/)** have been used. The 0.01" tool is necessary to isolate the TMC2660 pads, plus some other small details around it. Being a double sided board, it is necessary to use a double sided copper clad (FR1 or FR2 recommended for the milling), and to flip the board using fixed reference points. The board shown in this repository have been milled using a Roland SRM-20.

#### 1st step - mill the top traces

The first step is to mill the top traces using the 1/64 tool. Because the overall milling process is a long one with multiple steps and tools changing, it is recommended to note down the machine coordinates at the XY origin point.  Also to drill a small hole by manually starting the spindle in the origin point could help in case the machine coordinates are not reliable/lost. These tricks could allow you to resume the job in case something happens. For this step, Fabmodules with the standard settings for 1/64 process have been used.

Here is the top traces png requiring the 1/64 tool:
<img src="milling/top_traces.png" width="100%">

#### 2nd step - mill the top traces requiring the 0.01"

To avoid really long milling time, the areas requiring the 0.01" tool have been cropped from the top traces png, keeping the same png size to match the X and Y reference. These areas are only on the top layer of the PCB. The 0.01" tool is quite fragile, and for this  is recommended to use it only with the 0.01" process of the Fab Modules, and to even decrease a bit the speed to 1.5 or 1mm/s. Before launching the job remember to take again the Z reference after changing the tool.

Here the cropped image for the tiny details requiring the 0.01" tool:
<img src="milling/top_traces_001.png" width="100%">

#### 3rd step - mill the holes

Requiring the 1/32 tool, this step uses the 1/32 process from Fab Modules with standard settings. Pretty much following the standard way of doing it. Again before launching the job remember to take again the Z reference after changing the tool.

This is the png for the holes:
<img src="milling/top_holes.png" width="100%">

#### 4th step - mill the reference holes

The milling of the reference holes is done separately from the PCB holes because of the different cutting depth. If you don't mind about this and have slightly longer time for milling the holes, you could also join the 3rd and the 4th step. The used tool is again the 1/32, same as the process in Fabmodules. The recommended depth is 2.9mm (cutting length of the 1/32 is 3.175mm). In case you'll make yourself a double sided board remember that these holes must be exactly symmetric to the design of the board.

Use this png for the reference holes:
<img src="milling/top_reference_holes.png" width="100%">

#### 5th step - mill the board outline

In order to flip the board we now need to mill the board cutout. Similar to the job to cut the holes, this step still uses the 1/32 tool and default process from Fabmodules.

Here the board cutout png:
<img src="milling/top_cutout.png" width="100%">

#### 6th step - flip the board and mill the bottom layer

Is now time to remove the board and to flip it to mill the bottom traces. Try to do this as careful as possible to avoid scratching the bottom layer of copper. If you use a screwdriver, possibly try to put in between a piece of tissue. To place the board as precise as possible to match the previous origin point, use four M3 screws (or any other short 3mm cylinder). Use double side tape to fix the board to the sacrificial layer. To be noted the image is mirrored, and the reason is because you flipped the board. The bottom can be milled with the 1/64 tool and the 1/64 process from Fabmodules with standard settings.

Here is the png for the bottom traces:
<img src="milling/bottom_mirrored.png" width="100%">

Because of the difficulty of this step, below some images showing how I did it for the satstep2660 board shown in this repository. This how the sacrificial layer looks like after milling the PCB holes, the reference holes and the cutout:

<img src="media/sacrificial_after.jpg" width="100%">

Here board with attached 3mm rivets (used in this example, M3 screws will also fit). Pictures taken for the bottom, top and top with the double side tape applied:

<img src="media/reference cyl 1.jpg" width="100%">

<img src="media/reference cyl 2.jpg" width="100%">

<img src="media/tape_top.jpg" width="100%">

And finally the board fixed to the sacrificial layer with the rivets in place. The rivets (or the screws) can be removed from the board after fixing the board by pushing on it so that the double side tape will be attached. This to avoid any possibility the tool could crash into the rivets/screws.

<img src="media/bottom_attached.jpg" width="100%">

<img src="media/milling_bottom.jpg" width="100%">


Soldering tips
--

The satstep2660 board has been made by milling two sides on a double sided copper clad. In this case the vias just consist of cnc milled holes on the PCB, without any kind of internal coating. The way I to connect them make use of pinheaders passing trough and then by soldering them on both sides of the PCB. Multiple vias per connection and standard size pinheaders have been used to ensure enough conductivity for high current to pass without heating. Once soldered, the extra length of the pins used for the vias has been cut away.

<img src="media/vias_bottom.jpg" width="100%">

<img src="media/vias_top.jpg" width="100%">

The chip has the motor channels connected by using two pads simultaneously. Don't be afraid then that those are shorted, vice-versa, try to put as much soldering as possible to ensure the low resistivity of the connection.

Media
--
**satstep2660 dynamic microstepping Nema 24 test**:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=kPX6UlxPLYU
" target="_blank"><img src="http://img.youtube.com/vi/kPX6UlxPLYU/0.jpg" 
alt="http://img.youtube.com/vi/kPX6UlxPLYU/0.jpg" width="800" height="600" border="0" /></a>


**satstep2660 pictures**:

<img src="media/front2.jpg" width="100%">

<img src="media/top1.jpg" width="100%">

<img src="media/side2.jpg" width="100%">

<img src="media/back.jpg" width="100%">

<img src="media/noheat2.jpg" width="100%">

satstep2660 with 2 early test PCBs:

<img src="media/previous.jpg" width="100%">

satstep2660 compared with the satstep6600:

<img src="media/comparison1.jpg" width="100%">

<img src="media/comparison2.jpg" width="100%">

Author
--

**Daniele Ingrassia**

Contact
--
- **ingrassiada@gmail.com**
- **[linkedin](http://it.linkedin.com/in/danieleingrassia)**


Thanks
--
**[Fablab UAE](http://fablabuae.ae/)**<br />
176 6 D St - Satwa <br />
Dubai - United Arab Emirates<br />
fablabuae@gmail.com

License
-- 
This work is licensed under the terms of the open source license: Creative Commons Attribution-ShareAlike 4.0 International ([CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)).

Disclaimer  
--

<div class="align-justify">
This hardware/software is provided "as is", and you use the hardware/software at your own risk. Under no circumstances shall any author be liable for direct, indirect, special, incidental, or consequential damages resulting from the use, misuse, or inability to use this hardware/software, even if the authors have been advised of the possibility of such damages.</div>
