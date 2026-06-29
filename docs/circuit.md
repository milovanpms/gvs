# ⚙️ Circuit Design

## Overview

The circuit is a **transconductance amplifier**: it converts an input voltage (the joystick position) into a controlled output current through the electrodes. The core of the circuit is an **improved Howland current source**, preceded by an input signal generation stage and followed by a push-pull output stage with hardware current limiting.

The signal flows as follows:

1. A **stable `±2.5 V` supply** is generated from the `±9 V` rails
2. The **joystick position** sets a DC bias between `-2.5 V` and `+2.5 V` through a potentiometer
3. A **fade-in/fade-out RC filter** smooths abrupt transitions
4. A **Wien oscillator** generates a `300 mVp` sine wave at `1.5 Hz`
5. Both signals are **summed and inverted** before entering the Howland stage
6. The **Howland current source** converts the input voltage to a proportional current
7. A **push-pull stage** boosts output compliance and current capability
8. A **hardware current limiter** caps the output at `3–5 mA`
9. Current flows through the **electrodes** placed on the mastoid processes

## Power Supply

The circuit runs on a dual `±9 V` battery supply. Two additional stable references are derived from these rails for the input stage.

> ### Why do we need these references?
> The joystick control signal must swing symmetrically around `0 V` to allow
> bidirectional stimulation (tilt left and right). A potentiometer alone cannot
> generate a stable bipolar reference: we need two clean voltage
> references at `+2.5 V` and `-2.5 V` to feed its terminals.

**+2.5 V reference**: A `LM7805` regulator takes `+9 V` as input and outputs `+5 V`. This is fed into a `10 kΩ / 10 kΩ` voltage divider, producing `+2.5 V`, buffered by a `MCP6021` voltage follower (powered at `+Vcc = +5 V / -Vcc = GND`).

**-2.5 V reference**: A `LM7905` regulator takes `-9 V` as input and outputs `-5.31 V` (according to simulation). This is fed into a `11.255 kΩ / 10 kΩ` voltage divider, producing `-2.5 V`, buffered by a `MCP6021` voltage follower (powered at `+Vcc = GND / -Vcc = -5.31 V`).

> [!NOTE]
> I know, the voltage divider is really crappy. I use it to get a reasonably clean `-5 V` signal, for **simulation purposes**, but this needs to be refined in prototyping.
