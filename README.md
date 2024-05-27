# FMOD

A flexible (literally) interface for connecting FPGA modules.

Essentially PMOD reimagined with the 2x6 2.54mm pitch header replaced with a 20-pin 0.5mm pitch FFC connector.

## Pinout

| Function | Pin | Pin | Function |
|----------|-----|-----|----------|
| GND      |   1 |   2 | IO0      |
| GND      |   3 |   4 | IO1      |
| GND      |   5 |   6 | IO2      |
| GND      |   7 |   8 | IO3      |
| GND      |   9 |  10 | VIO      |
| VIO      |  11 |  12 | GND      |
| IO4      |  13 |  14 | GND      |
| IO5      |  15 |  16 | GND      |
| IO6      |  17 |  18 | GND      |
| IO7      |  19 |  20 | GND      |

## Connector

The recommended connector is Omron XF3M-2015-1B, but any compatible 20-pin 0.5mm pitch connector can be used.
One benefit of the recommended connector is that it has both top and bottom contacts, so it'll work with both cable orientations.

## Cable type/orientation

The nominal cable type is a top/bottom cable, inserted contacts up on either end, making a 1:1 connection.
However, the pinout is designed to be symmetric, meaning that with a reversal of the signal order in the FPGA gateware, a mirrored connection can also be made when appropriate, e.g. to ease cable routing.

## Background

I came up with this while building a replacement controller for a three decades old FANUC robot arm.
Since I was reverse-engineering the various interfaces, I wanted a modular design so I wouldn't have to respin the entire thing if I got one of the interfaces wrong, so I needed a way to distribute 70-80 signals between a FPGA board and four different interface boards, with a varying number of signals going to each.

## FAQ

### What is VIO?

The IO voltage of the signals.
Usually 3.3V, but other IO voltages are allowed.
Nominally provided by the FPGA side and consumed by the peripheral side, but variations are allowed.
The integrator is responsible for ensuring that the voltage, current and direction specs of both sides of a connection are compatible.

### Why no other voltage rails?

Simplicity.
Also, having only a single voltage rail allows 1:1 adapters between PMOD and FMOD.

A common IO voltage is almost always required, whereas which other voltage rails exists/is required would be fairly application specific and would make for a complex compatibility matrix.

### Why only eight signals?

Because eight is a good granularity when you're dividing the available FPGA IOs between multiple modules.
If you're making a module that needs e.g. only five signals, you're only wasting three.
If you're making a module that needs more than eight signals, you just run multiple FMOD interfaces in parallel.

### Why so many grounds?

Surrounding every signal with grounds makes for good signal integrity and 20-pin FFC-connectors and cables is also a common size with good availability.
There would be no significant saving from shaving off the number of grounds and switching to a lower pin count.

## Projects with FMOD interfaces

- [zyp's replacement controller for a FANUC R-J2](https://bin.jvnv.net/file/aizay.jpg)

If you're using FMOD in your project and would like to see it listed here, please open an issue or a pull request.
