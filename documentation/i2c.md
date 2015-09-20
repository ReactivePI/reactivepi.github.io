---
layout: documentation
title: Working with I2C
---
The reactive PI library contains a set of components that you can use to access the I2C bus on your raspberry PI. This guide describes how to use the I2C class and its companions to work with the I2C port.

## Getting started
**Important** Before you can use the I2C bus on your raspberry PI, you have
to enable it. To do this check the guide [activate I2C](/documentation/activate-i2c.html).

To start using the I2C bus, you create a new instance of the bus you want to use and ask for a device on a specific address. The following sample demonstrates this:

``` scala
// 0x1 = bus number, 0x12 = address of the device
val device = I2CDevice.open(0x1, 0x12)

// Write and read data from the device directly.
device.write(Array[Byte](0xf))
val data = device.read(1)
```

To find out the address of a device on the I2C bus, you can use the tool `i2cdetect` which is part of the the i2c-tools package that you can install on your raspberry PI. To find out more, check out [this guide](http://www.lm-sensors.org/wiki/man/i2cdetect)

## Reading/Writing from/to a specific register on a device
Some I2C devices, such as extension boards come with the possibility to write to specific addresses on the device. In the case of extension boards this is useful, because you are first addressing the extension board and then addressing a device on the board.

To use an extension board or any other device that supports registers, you can use
the following sample code:

``` scala
// Read and write data from a device that uses device registers.
device.write(0x1, Array[Byte](0xf,0xf))
val data = device.read(0x1, 2)
```

## Managing OS resources
It is important to call the `close()` method on the I2Device. Not doing this will
result in an unusable I2C bus. Restarting the Raspberry PI will fix the problem, but
it is far from ideal.

## Working with actors
Reactive PI supports the use of actors for communication over I2C.
To use actors instead of the regular `I2CDevice` class you need to create a new
I2C actor. The following sample demonstrates this:

``` scala
class I2CSamples extends Actor {
  val device = I2C(1).device(0)

  device ! I2C.Write(Array(1))
  device ! I2C.Read(1)
}
```

If you need to communicate with a I2C device that uses device registers, you need to add
an extra parameter to the messages.

``` scala
class I2CSamples extends Actor {
  val device = I2C(1).device(0)

  device ! I2C.Write(5, Array(1))
  device ! I2C.Read(5, 1)
}
```

The first parameter in the Write and Read message for I2C is optional and defaults to -1, meaning it will not use a register to write to. When you want to write to a specific register, you explicitly provide a value for the first parameter in the message.

## Want to know more?
There's some general information on the I2C driver contained in the Raspbian distribution of Linux on the kernel pages. Check this URL for more info: https://www.kernel.org/doc/Documentation/i2c/dev-interface
