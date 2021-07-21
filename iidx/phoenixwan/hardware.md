# Phoenixwan Hardware

## Turntable sensors

The Phoenixwan uses two LITEON LTH-301-32 sensors to detect the encoder wheel. This sensor does not have any built-in electronics; it is simply an LED and a photodiode.

On the board in the Phoenixwan, a built-in resistor (470 ohm) limits the current to the LED to approximately 9 mA.

It is possible to read the signal directly using a microcontroller, without a schmitt trigger or comparator to clean the signal. A pull-up resistor is necessary, as the photodiode will only be able to pull down.