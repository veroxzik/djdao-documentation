# Phoenixwan Turntable LED Ring

The turntable LED ring is a standalone unit, featuring its own microprocessor to control the lights. It receives communicates via TTL level RS-232, at 115200 baud, 8 data bits, 1 stop bit, no parity, and no flow control. No line endings are required. Data is constantly being transmitted by the turntable microprocessor, so be sure to receive and flush as necessary.

### Abbrevations and Byte Convention

For the purpose of this README, I'll use the following abbreviations:

| Abbreviation | Item |
|:---:|:---:|
| MB | Motherboard |
| TT | Turntable | 
| CW | Clockwise |
| CCW | Counter-Clockwise |

Bytes will be written as hex, followed by the ASCII equivalent in parenthesis, if relevant. Example: `0x5A ('Z')`.

## Initialization Sequence

At startup, the TT spams `0x5A` until the MB responds.

The initialization is as follows:

1. TT spams `0x5A`
2. MB sends `0x5A`
3. TT sends `0x7E`
4. MB sends `[Byte 0]`
5. TT sends `[Byte 0]` with the upper nibble XOR'd with 5 and the lower nibble inverted
6. MB sends `[Byte 1]`
7. TT sends `[Byte 1]` with the upper nibble XOR'd with 7 and the lower nibble XOR'd with 8
8. MB sends `[Byte 2]`
9. TT sends `[Byte 2]` with the upper nibble XOR'd with 5 and the lower nibble XOR'd with 5
10. MB sends `[Byte 3]`
11. TT sends `[Byte 3]` with the upper nibble XOR'd with 7 and the lower nibble XOR'd with 4
12. MB sends `[Byte 4]`
13. TT sends `[Byte 4]` with the upper nibble XOR'd with 10 and the lower nibble inverted
14. MB sends `[Byte 5]`
15. TT sends `[Byte 5]` with the upper nibble inverted and the lower nibble XOR'd with 8
16. MB sends `0xA5` to confirm the initialization process has been completed

The TT will resume the last mode it was in before powering off.

## Modes

After initialization, the TT spams what mode it is currently in, as `0x31 ('1')` to `0x37 ('7')`.

The modes, per Gamo2's website, are listed in the following table.  
Options refers to whether the Hue (H), Saturation (S), or Brightness (B) are user-defined.

| Mode # | Name | Options | Description | TT Effect |
|:---:|:---:|:---:|:---|:---|
| 1 | Constant Light Mode | HSB | Outputs a constant light at the user-defined hue, saturation, and brightness. | TT does nothing. |
| 2 | Marquee Mode | H | Outputs a steady rotation CW around the TT at the user-defined hue. | Spins faster in the same direction the TT is spun for a short while. |
| 3 | Rainbow Automatic Transformation | SB | Very slowly fades between hues. | TT does nothing. |
| 4 | Rainbow Fixed Color | SB | Output a static rainbow. | TT does nothing. |
| 5 | Rainbow Synchronous Rotation | SB | Outputs a rainbow that spins with TT movement. | Spins in the direction the TT is spun. Gets faster when spun faster. |
| 6 | Trigger Mode | HS | Outputs a dim light when TT is stationary and gets brighter when TT is spun. | Gets brighter when TT is spun. |
| 7 | Breathing mode | HS | Slowly fades in and out at the user-defined hue. | TT does nothing. |

To set the TT to a certain mode, send the mode in ASCII (e.g. `0x31 ('1')` to `0x37 ('7')`).

## Changing Colors

In some modes, it is possible to set the Hue (H), Saturation (S), or Brightness (B).  
Refer to the table above to note which commands work in which modes.

| Command | Action |
|:---:|:---|
|`0x91` | Reduce Hue |
|`0x81` | Increase Hue |
|`0x92` | Reduce Saturation |
|`0x82` | Increase Saturation |
|`0x93` | Reduce Brightness |
|`0x83` | Increase Brightness |

## Additional Mode Commands

### Mode 1: Constant Light Mode

This mode does not accept any additional commands.

### Mode 2: Marquee Mode

| Command | Action |
|:---:|:---|
|`0x00` | TT has stopped spinning |
|`0x4E ('N')` | TT spun CW |
|`0x4F ('O')` | TT spun CCW |

### Mode 3: Rainbow Automatic Transformation

This mode does not accept any additional commands.

### Mode 4: Rainbow Fixed Color

This mode does not accept any additional commands.

### Mode 5: Rainbow Synchronous Motion

**NOTE**: Most of the higher speeds are quite indistinguishable from each other. I'm not sure why there's so many.

| Command | Action |
|:---:|:---|
|`0x00` | TT has stopped spinning |
|`0x51 'Q'` | CW (speed 1) |
|`0x52 ('R')` | CW (speed 2) |
|`0x53 ('S')` | CW (speed 3) |
|`0x54 ('T')` | CW (speed 4) |
|`0x55 ('U')` | CW (speed 5) |
|`0x56 ('V')` | CW (speed 6) |
|`0x57 ('W')` | CW (speed 7) |
|`0x58 ('X')` | CW (speed 8) |
|`0x59 ('Y')` | CW (speed 9) |
|`0x5A ('Z')` | CW (speed 10) |
|`0x5B ('[')` | CW (speed 11) |
|`0x5C ('\')` | CW (speed 12) |
|`0x5D (']')` | CW (speed 13) |
|`0x5E ('^')` | CW (speed 14) |
|`0x5F ('_')` | CW (speed 15) |
|`0x61 ('a')` | CCW (speed 1) |
|`0x62 ('b')` | CCW (speed 2) |
|`0x63 ('c')` | CCW (speed 3) |
|`0x64 ('d')` | CCW (speed 4) |
|`0x65 ('e')` | CCW (speed 5) |
|`0x66 ('f')` | CCW (speed 6) |
|`0x67 ('g')` | CCW (speed 7) |
|`0x68 ('h')` | CCW (speed 8) |
|`0x69 ('i')` | CCW (speed 9) |
|`0x6A ('j')` | CCW (speed 10) |
|`0x6B ('k')` | CCW (speed 11) |
|`0x6C ('l')` | CCW (speed 12) |
|`0x6D ('m')` | CCW (speed 13) |
|`0x6E ('n')` | CCW (speed 14) |
|`0x6F ('o')` | CCW (speed 15) |

### Mode 6: Trigger Mode

| Command | Action |
|:---:|:---|
|`0x00` | TT has stopped spinning. This command appears to be useless because the trigger times out by itself. |
|`0x71 ('q')` | TT moved (either direction) |

### Mode 7: Breathing mode

This mode does not accept any additional commands.