# RC Aircraft Wing and Body Lights

An Arduino-controlled, realistic navigation and anti-collision (strobe) lighting system for a paper/RC aircraft model. Steady red position light on the left wingtip, steady green on the right wingtip, plus synchronized double-flash strobe lights on the wing and body.

## Parts Used

- Arduino Uno
- 3x Red LED (position light)
- 1x Green LED (position light)
- 1x Yellow LED (wing strobe)
- 1x Yellow LED (body strobe)
- Resistors (~220-330 ohm, one per LED)
- Breadboard and jumper wires

## Pin Connections

| Pin | Role | Behavior |
|---|---|---|
| 8 | Body strobe (bodyYellow) | Double flash (strobe) |
| 9 | Wing strobe (wingYellow) | Double flash (strobe) |
| 10, 11, 12 | Red position lights (redPins) | Steady on |
| 13 | Green position light (greenPin) | Steady on |

The wiring diagram is included as `ARDUINO_CIRCUIT_DIAGRAM.png`.

## How It Works

The `loop()` function continuously repeats this sequence:

1. **Wing strobe** (pin 9) does a double flash: 40ms on → 100ms off → 40ms on → 500ms off
2. **Body strobe** (pin 8) does a double flash: same timing (40-100-40-500 ms)
3. The sequence repeats from the start

The red and green position lights (`redPins`, `greenPin`) are set to `HIGH` once in `setup()` and never changed in `loop()` — they stay on continuously.

## Adjusting the Timing

To change the flash duration or the wait between flashes, edit the `delay()` values in `loop()`:

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

The wing and body strobes can be adjusted independently in their own `wingYellow` and `bodyYellow` blocks.

## Changing Pins

To use different pins, just update the variables at the top of the file:

```cpp
int wingYellow  = 9;
int bodyYellow  = 8;
int redPins[]   = {10, 11, 12};
int greenPin    = 13;
```

## Notes

- Every LED needs a matching resistor (~220-330 ohm).
- The `redPins` array drives 3 LEDs together, all staying steadily on at the same time.
- The project is written using `delay()` to keep the code simple and easy to read for beginners.
