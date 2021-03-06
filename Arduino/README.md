﻿# Remote Arduino Provider
This sample demonstrates how to replace the default bus implementations (for GPIO, I2C, PWM, and ADC) with implementations leveraging the remote-wiring libraries.  These libraries provide an easy interface for controlling a remote Arduino.

## Using the ArduinoProviders library and sample code
This sample library provides a set of Windows.Devices.*Providers WinRT APIs enabling apps to use remote-wiring's communication with and control of GPIO, I2C, PWM, and ADC devices on a remote Arduino.
The current version includes:
* ArduinoProviders.ArduinoProvider: Implements ILowLevelDevicesAggregateProvider and is the top level API to enable retrieving controller providers for GPIO, I2C, PWM, and ADC.

## API Usage
The general pattern of this sample provider (or any other WinRT component implementing the Windows.Devices.*Provider interfaces) is to retrieve the top level controller from the provider and then use the controller the same way as the default controllers.

The easiest way to use these providers is to configure the Arduino properties (VID, PID, and baud rate) and set the ArduinoProvider as the default provider for an application. The code below will set it as the default provider. 
```C#
ArduinoProviders.ArduinoProvider.Configuration = 
    new ArduinoProviders.ArduinoConnectionConfiguration("VID_2341", "PID_0043", 57600);
Windows.Devices.LowLevelDevicesController.DefaultProvider =  new ArduinoProviders.ArduinoProvider();

gpioController = await GpioController.GetDefaultAsync();
i2cController = await I2cController.GetDefaultAsync();
adcController = await AdcController.GetDefaultAsync();
pwmController = await PwmController.GetDefaultAsync();
```

## Build Requirements
This sample provider library depends on the remote-wiring code (https://github.com/ms-iot/remote-wiring).  To run this sample, you will need to:

1. Clone the complete BusProvider repository in Visual Studio 2017 with the "Recursively Clone Submodules" option selected. Alternatively you can use the Git command line tools to clone the repository but you will need to use the "--recursive" option.
2. Ensure that your Arduino is configured propertly for remote-wiring (https://github.com/ms-iot/remote-wiring/blob/develop/README.md#arduino-setup).

## Hardware Requirements

The ArduinoConsumer and ArduinoConsumerCpp projects utilize the ADC, PWM, GPIO, and I2C buses to drive a "thermostat driven fan."  To configure this, you'll need the following hardware:

* Arduino board (UNO is used in this sample)
* Servo (driven by PWM)
* Potentiometer (read via ADC)
* HTU21D temperature/humidity sensor (read via I2C)
* LED (driven by GPIO)
* 220 Ohm resistor
* Pushbutton (driven by GPIO)
* 10k Ohm resistor
* breadboard and some wires

Setup your "thermostat driven fan" according to this diagram:

![HW Setup](https://github.com/ms-iot/BusProviders/blob/develop/Arduino/ArduinoConsumer_bb.jpg).

### SDK version
Additionally, the following version of the Windows SDK should be available for building and using the library: 10.0.10563.0

