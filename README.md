# TDC Test
A serial port command line test application for Maxim's MAX3510x time-to-digitial converters.  

## Overview

This firmware can be used with the MAX32625MBED board mated to a MAX35104EVKIT2 Arduino shield.  The purpose of the firmware
is not only to provide a register-level interface to the MAX35104, but also to serve as a code reference and starting point for 
prototyping work.

The MAX32625MBED board:  https://www.maximintegrated.com/en/products/microcontrollers/MAX32625MBED.html

The MAX35104EVKIT2 board:  <comming soon>

![alt text](https://github.com/maxim-ic-flow/tdc_test/blob/master/readme_images/max32625mbed_max35104.jpg "MAX32625MBED + MAX35104EVKIT2")

The serial port interface on the MAX32625MBED HDK USB port is used to provide a command-line like
interface for configuring and testing the MAX35104 TDC.

## Repository

Please note that this project uses git submodules.  The proper way to clone this repository is as follows:

```
git clone --recursive https://github.com/maxim-ic-flow/tdc_test.git
```
To switch between branches:

```
git checkout <branch>
git submodule update --recursive --remote --init
```

## Tools

<b>Keil uVision V5.23.0.0+</b>
<p>http://www2.keil.com/mdk5

<b>Rowley CrossWorks for ARM V4.1+</b>
<p>https://www.rowley.co.uk/arm/index.htm

<b>Makefile with user supplied ARM GCC toolchain</b>

## Building

The makefile in the GCC directiory can be used to generate elf and bin images with the user provided toolchain.

```
make [clean|release]
```

In the case of a release build, the resulting tdc_test.bin file can be dragged and dropped onto the DAPLINK mass
storage device provided by the MAX32625MBED HDK USB interface.

The project files for uVision and CrossWorks are in the keil and cw subdirectories and can be built and programmed
as normal for those platforms.  Note that the MAX32625MBED board provides a CMSIS-DAP SWD debugging interface.
You must conifugre your debugger accordingly.

## Running tdc_test

Once the image is programmed, you can use PuTTY or similar serial port tool to connect with the MAX32625MBED board.

![alt text](https://github.com/maxim-ic-flow/tdc_test/blob/master/readme_images/putty.jpg "Serial port command line interface via PuTTY")

type 'help' for a list of all commands.  Most of the commands mirror TDC registers.  Refer to the MAX35104 datasheet for details.

https://datasheets.maximintegrated.com/en/ds/MAX35104.pdf

Many register-specific commands can take arguments.  For example to set the TDC delay register, you can use:

```
> dly=200
```

To query the curreent delay register value, use:

```
> dly?
```

The 'help' command provides basic usage information.  See the source for more specific usage and functional information.

## Quick Start

The 'tof_diff' command will perform a basic bi-direction time measurement.  Using this command along with an oscilloscope will allow you to inspect
the relevant signals on the MAX35104EVKIT2 Arduino shield in order to start dialing in your transducer configuration.  The result of a successful
measurement looks like this:

![alt text](https://github.com/maxim-ic-flow/tdc_test/blob/master/readme_images/tof_diff.jpg "TOF_DIFF command results")

Be sure that your mode=idle when executing individual TDC commands (see next section).

## Modes of operation

tdc_test provides three modes of operation:

<p>idle - no automated TDC command sequencing.  This allows the user to issue individual TDC commands and immediately see the results.
<p>host - the host processor provides command timing via the 'sampling' command.  This allows higher sampling rates that can be achieve in event mode.
<p>event - uses the event processor inside the MAX35104 to perform TDC command sequencing

The mode of operation can be selected with the 'mode' command.

```
> mode=idle
> mode=host
> mode=event
> mode?
```

</b>Other non-register commands:

<p><b>spi_test</b> - helps with basic SPI bus debugging.
<p><b>tof_temp</b> - specifies the number of Time-Of-Flight commands per temperature command when in 'host' mode.
<p><b>default</b>  -  restore defaults as described in transducer.c
<p><b>sampling</b> - frequency (Hz) of sampling when in 'host' mode.
<p><b>report</b>   - dumps the contents of the hit registers in both directions and the temperature registers.  Useful for data collection.
<p><b>dc</b>       - dumps the value of all settings for easy inspection

## Related Tools

The MAX35104EVKIT provides a high-level Windows GUI for interacting with the MAX35104 chip without the need to compile firmware.

https://www.maximintegrated.com/en/products/analog/data-converters/analog-front-end-ics/MAX35104EVKIT.html


Note that the MAX35104EVKIT is not earlier version of the MAX35104EVIT2 described here.  It is a seperate kit with different capabilities
may be more appropriate for initial evaluation.




