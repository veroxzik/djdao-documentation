# Phoenixwan Center Light Bar

The center light bar is a standalone unit, featuring its own microprocessor to control the lights. It receives communicates via TTL level RS-232, at 115200 baud, 8 data bits, 1 stop bit, no parity, and no flow control. No line endings are required. 

### Abbrevations and Byte Convention

For the purpose of this README, I'll use the following abbreviations:

| Abbreviation | Item |
|:---:|:---:|
| MB | Motherboard |
| LB | Light Bar | 

Bytes will be written as hex, followed by the ASCII equivalent in parenthesis, if relevant. Example: `0x5A ('Z')`.

## Initialization Sequence

At startup, the LB spams `0xF0`. Once the MB replies with `0xA5`, the LB will respond with `0x5A` to acknowledge the initialization.

## Changing Colors

It is possible to set the Hue (H), Saturation (S), or Brightness (B) of the LB using the following commands:

| Command | Action |
|:---:|:---|
|`0x91` | Reduce Hue |
|`0x81` | Increase Hue |
|`0x92` | Reduce Saturation |
|`0x82` | Increase Saturation |
|`0x93` | Reduce Brightness |
|`0x83` | Increase Brightness |

## Light Commands

The LB has 9 possible commands to change the number of LEDs lit up.

| Command | Action |
|:---:|:---|
|`0x4C` | LEDs Off |
|`0x68` | Center 2 LEDs On |
|`0x69` | Center 4 LEDs On |
|`0x6A` | Center 6 LEDs On |
|`0x6B` | Center 8 LEDs On |
|`0x6C` | Center 10 LEDs On |
|`0x6D` | Center 12 LEDs On |
|`0x6E` | Center 14 LEDs On |
|`0x6F` | Center 16 LEDs On |