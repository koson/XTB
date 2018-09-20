## Xlab Tester Board - 2.1 (XTB-20)
Open-source in-situ device driver & data acquisition board for use in Stanford's eXtreme Environment Microsystems Laboratory [xlab.stanford.edu](https://xlab.stanford.edu/)
* Design centers around TI's multi-functional 24bit ADC [ADS124S08](http://www.ti.com/product/ADS124S08)
* 4-layer PCB can be ordered at OSH Park: [https://oshpark.com/shared_projects/32BmjtOQ](https://oshpark.com/shared_projects/32BmjtOQ)

<img width="800" src="https://github.com/maholli/XTB/blob/master/media/Intro.PNG">

<p align="middle">
  <img width="700" src="https://github.com/maholli/XTB/blob/master/media/integration.PNG">
</p>

## Datalogging with the XTB
Operating, measuring, and saving data with the XTB boards is handled by a microcontroller. Nearly any microcontroller will work, and the code is written in Arduino to aid in use. The example below uses an [Adafruit Feather M0 with microSD card](https://www.adafruit.com/product/2796) for easy datalogging capability. 

<p align="middle">
  <img width="1000" src="https://github.com/maholli/XTB/blob/master/media/XTB_wiring.PNG">
</p>

### Configuration and Operation
As long as you're using one of the XLab Adafruit Feathers floating around, there is no additional software needed to configure and run tests using the XTB. This is accomplished by editing the "config.txt" text file stored on the microSD card.

How it works:
1. Every time the XLab Adafruit Feather board is powered on, it looks for the "config.txt" file.
  * If the SD card isn't installed, the config.txt file isn't present, or the Feather isn't able to write to the SD card, the red LED will illuminate. No actions are taking place while the red LED is on. It will stay on until the power source is removed from the Feather board.
2. Once the Feather finds "config.txt" it will read each line in the text file and execute specific commands that it finds.
  * The "config.txt" commands are very specific and listed below (don't include the brackets). Only one command per line, each command must finish with a comma, commands are case-sensitive, there is a space between arguments, and the last line of the file must be blank. 
    * `start,` tells the ADC to start take measurements and take it out of 'standby' mode
    * `stop,` tells the ADC to stop take measurements and puts it in 'standby' mode
    * `reset,` resets the ADC to a preconfigured state
    * `delay [wait time (in ms)],` tells the Feather to wait for specific amount of time
    * `temp [save data? options: "t" or "f"],` tells the ADC to make a temperature measurement and (depending on the argument), whether or not to save the data to the SD card
    * `read [+ pin to read] [- pin to read] [IDAC pin] [VBias Pin] [save data options: "t" or "f"] [wait time (in ms)] [# of measurements to save] [IDAC mag] [data label text],` tells the ADC to configure itself according to the arguments and then instructs the ADC to make a series of voltage measurements using the new configuration
      * `[+ pin to read]` accepts: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 (12 is GND)
      * `[- pin to read]` accepts: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 (12 is GND)
      * `[IDAC pin]` accepts: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
      * `[VBias Pin]` applies 0.275V to specified pin:
        * 1 (AIN0)
        * 2 (AIN1)
        * 3 (AIN0 and AIN1)
        * 4 (AIN2)
        * 5 (AIN0 and AIN2)
        * 6 (AIN1 and AIN2)
        * 8 (AIN3)
      * `[IDAC mag]` sets the current source level *MUST HAVE A PATH TO GROUND*
        * 1 10 uA
        * 2 50 uA
        * 3 100 uA
        * 4 250 uA
        * 5 500 uA
        * 6 750 uA
        * 7 1000 uA
        * 8 1500 uA
        * 9 2000 uA
    * `batt,` measures the battery voltage of the Feather board and saves it to the SD card
    * `readout,` tells the ADC to print the state of all of its registers over the serial port - see TI spreadsheet to decode register values
    * `calibrate,` tells the ADC to perform a self calibration and adjust the offset registers accordingly - usually followed by a readout command
    * `gpio [which GPIO pins to enable] [what state to set the gpio pins],` tells the ADC to configure the gpio registers accordingly - argument 1 is a decimal input for the 0x10 register (GPIODAT) and argument 2 is a decimal input for the 0x11 register (GPIOCON)
    
<p align="middle">
  <img width="200" src="https://github.com/maholli/XTB/blob/master/media/configExample.PNG"><br>Simple config.txt example file that starts the XTB, makes a temperature measurement (saves it to the SD card), delays for 100ms, and then repeats.
</p>
<br>
<br>
<p align="middle">
  <img width="300" src="https://github.com/maholli/XTB/blob/master/media/configExample2.PNG"><br>More complex config.txt example file that performs measurements on multiple devices and saves to the SD card.
</p>
