# SVRE9 LED Boards

The LED boards that come stock with the SVRE9 have fairly minimal functionality.

## Colors

The board supports 7 preset colors.

Upon power up, the board will light up in the last color that was set.

| Color Order |
|:---:|
| Red |
| Green |
| Blue |
| Yellow |
| Pink |
| Cyan |
| White |

## Connector

The connector on the board is a 3-pin JST XH, P/N B3B-XH-A and the mating connector would be P/N XHP-3. Wire colors may vary.

Pin 1 is the left-most pin when viewing the drawing below.

| Pin # | Function |
|:---:|:---:|
| 1 | GND |
| 2 | +5V |
| 3 | Toggle |

## Signals

`Toggle` by default should be held at `+3.3V`. When a color change is desired, bring `Toggle` to `0V`.

Using `+5v` for `Toggle` will also work (verified by [CrazyRedMachine](https://github.com/CrazyRedMachine).)

The board will not respond faster than 20Hz, or 50ms, and even then it's somewhat inconsistent.

## Dimensions

For dimensions, see [the pdf in this folder](svre9-led-board.pdf).