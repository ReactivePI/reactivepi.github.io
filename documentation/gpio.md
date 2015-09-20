---
layout: documentation
title: Working with GPIO
---
The reactive PI library contains a set of components that you can use to access the GPIO port on your raspberry PI. This guide describes how to use the GPIO class and its companions to work with the GPIO port.

## Getting started
Basic operation of input and output pins on your Raspberry PI can be done by
creating a new GPIO input or output pin instance. The following sample demonstrates
the use of both:

``` scala
val input = GPIODevice.input(7)
val output = GPIODevice.output(13)

val x = input.read()
output.write(x)
```

### Opening and closing ports
When not working with actors you are responsible for opening and closing access
to GPIO pins yourself. It is important to call `close()` on the input and output pins
it leaves the Raspberry PI in an undefined state. Restarting the device is necessary
in this case.

## Working with actors
To start using the GPIO port through Akka actors,
you either have to create a new input or output actor in your application.
The following sample demonstrates both:

``` scala
import akka.actor.Actor
import reactivepi.GPIO

class GPIOSamples extends Actor {
  val input = GPIO.input(1)
  val output = GPIO.output(2)

  input ! GPIO.Read

  def receive = {
    case GPIO.Data(val) => output ! GPIO.Write(val)
  }
}

object GPIOSamplesApplication extends App {
  implicit val actorSystem = ActorSystem("gpio-controller")
  val actor = actorSystem.actorOf(Props(new GPIOSamples()))
}
```

This sample reads from the input by sending a `GPIO.Read` message to the input pin.
When the input pin has actually read the value it will send it back to the original actor (GPIOSamples).
The actor will then send a new `GPIO.Write` message to the output pin.

### Subscribing to an input port
You can read from an input port explicitly. There's also the option to subscribe to an input. This will enable a more event driven experience. To subscribe to an input pin use the following code:

``` scala
import akka.actor.Actor
import reactivepi.GPIO

class GPIOSubscription extends Actor {
  val input = GPIO.subscribe(3, self)
  val output = GPIO.output(4)

  def receive = {
    case GPIO.Data(val) => output ! GPIO.Write(val)
  }
}

object GPIOSamplesApplication extends App {
  implicit val actorSystem = ActorSystem("gpio-controller")
  val actor = actorSystem.actorOf(Props(new GPIOSubscription()))
}
```

Every time the input changes, the input actor will automatically send a `GPIO.Data` message to the GPIOSubscription actor, which will in turn send a `GPIO.Write` message to the output.

### Opening and closing ports when using actors
When using the actors API you don't need to open or close GPIO ports. When you invoke the GPIO.input, GPIO.output or GPIO.subscribe methods the port you want to access is automatically opened for you. When you stop the actor that you got when you invoked one of the methods on the GPIO component, the port you were using before is automatically closed for you.

## Want to know more?
There's some general information on the GPIO driver contained in the Raspbian distribution of Linux on the kernel pages. Check this URL for more info: [https://www.kernel.org/doc/Documentation/gpio/sysfs.txt](https://www.kernel.org/doc/Documentation/gpio/sysfs.txt)
