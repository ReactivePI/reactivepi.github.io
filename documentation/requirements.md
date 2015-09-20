---
layout: documentation
title: System requirements
---

Eventhough the code is written in Scala, a distribution of Linux with appropriate drivers installed is required in order for the library to correctly function.

Right now there is no detection that the drivers required are indeed available, so your application will crash when you try to use any of the components from the library on a system that hasn't got the correct drivers.

## Supported models
All models of the Raspberry PI are supported by this library.
Please let me know if you run into any problems with the driver on your Raspberry PI.

## Required drivers
Please make sure that your device has the correct drivers available:

 - GPIO Driver [https://www.kernel.org/doc/Documentation/gpio/sysfs.txt](https://www.kernel.org/doc/Documentation/gpio/sysfs.txt)
 - I2C Driver [https://www.kernel.org/doc/Documentation/i2c/summary](https://www.kernel.org/doc/Documentation/i2c/summary)


Please check the wiki for specific information on how to enable the I2C driver on your Raspberry PI as it requires manual intervention to get things working.
