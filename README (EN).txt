# RC Aircraft Wing and Body Lights

An Arduino-controlled realistic navigation and anti-collision (strobe) light system for a paper/RC aircraft model. Features steady red position light on the left wing, steady green position light on the right wing, and synchronized double-flash strobe lights on the wings and body.

## Components Used

- Arduino Uno
- 3x Red LED (position light)
- 1x Green LED (position light)
- 1x Yellow LED (wing strobe)
- 1x Yellow LED (body strobe)
- Resistors (~220-330 ohm, one per LED)
- Breadboard and jumper wires

## Pin Connections

| Pin | Function | Behavior |
|---|---|---|
| 8 | Body strobe (bodyYellow) | Double flash (strobe) |
| 9 | Wing strobe (wingYellow) | Double flash (strobe) |
| 10, 11, 12 | Red position lights (redPins) | Steady on |
| 13 | Green position light (greenPin) | Steady on |

The circuit diagram is available in `ARDUINO_CIRCUIT_DIAGRAM.png`.

## How It Works

The code repeats the following sequence continuously inside `loop()`:

1. **Wing strobe** (pin 9) performs a double flash: 40ms on -> 100ms off -> 40ms on -> 500ms off
2. **Body strobe** (pin 8) performs a double flash: same timing (40-100-40-500 ms)
3. The sequence repeats from the start

Red and green position lights (`redPins`, `greenPin`) are set to `HIGH` once in `setup()` and never changed in `loop()` -- meaning they stay lit continuously.

## Adjusting the Timing

To change the flash duration or wait times, edit the `delay()` values inside `loop()`:

```cpp
digitalWrite(wingYellow, HIGH);
delay(40);   // flash duration (ms)
digitalWrite(wingYellow, LOW);
delay(100);  // gap between the two flashes (ms)
digitalWrite(wingYellow, HIGH);
delay(40);   // flash duration (ms)
digitalWrite(wingYellow, LOW);
delay(500);  // wait before the next cycle (ms)
```

The wing and body strobe blocks (`wingYellow` and `bodyYellow`) can be adjusted independently using the same structure.

## Changing Pins

To connect the LEDs to different pins, just update the variables at the top of the file:

```cpp
int wingYellow  = 9;
int bodyYellow  = 8;
int redPins[]   = {10, 11, 12};
int greenPin    = 13;
```

## Notes

- Each LED must be connected with an appropriate resistor (~220-330 ohm).
- The three pins in `redPins` are all set to stay on together, at the same time.
- The project is written using `delay()`-based timing, keeping the code simple and easy to read.