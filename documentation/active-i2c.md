To use the I2C functionality in ReactivePI, you first need to enable the kernel module and install the necessary tools on your raspberry PI. Follow this guide if you haven't had I2C enabled on your Raspberry PI before.

## Activate I2C
To enable I2C, run the command `sudo raspi-config`.
Next go to the advanced menu and select I2C. Select yes to enable it and make sure that it is loaded by default.
After you are done, close the utility.

## Activate the necessary modules
Open up the file `/etc/modules` in your favorite editor and add the following lines to that file:

```
i2c-bcm2708
i2c-dev
```

Please make sure that you open the editor using sudo. Otherwise you will not be able to save the file ;-)
After you have activated the module, close the utility and reboot your Raspberry PI.

## Install the necessary tools
Now that you have the necessary modules and options activated you need to install the i2c-tools package. This package contains useful debugging tools that will make life a lot easier during development.

To install the debugging tools, use the following command:

```
sudo apt-get update
sudo apt-get install -y i2c-tools
```

That's it, you're good to go.
