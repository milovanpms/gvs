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

> ### Why is the MCP6021 powered at GND / -5.31 V here?
> The `MCP6021` requires the signal it buffers to fall within its supply voltage range.
> Here, the signal to buffer is `-2.5 V`, which is negative. If we
> powered the op-amp with the usual `+5 V / GND`, its output could never reach
> `-2.5 V` since `GND` would be its negative rail.
>
> By powering it at `+Vcc = GND` and `-Vcc = -5.31 V`, the op-amp can swing
> between `GND` and `-5.31 V`, which includes `-2.5 V`.

## Input Signal Generation

### Joystick Control Signal

The joystick position is translated into a DC voltage between `-2.5 V` and '+2.5 V` using a potentiometer fed by the two stable references:

| Potentiometer position | Output voltage |
|------------------------|---------------|
| `0.000` (full left) | `-2.5 V` |
| `0.500` (center) | `0 V` |
| `1.000` (full right) | `+2.5 V` |

> ### Why a fade-in / fade-out filter?
> A direct connection from the potentiometer to the Howland input would cause
> abrupt current jumps when the joystick is moved quickly, which could be
> uncomfortable or unsafe (*not likely but we never know*) for the user. A `RC` filter (`R = 22 kΩ`, `C = 10 µF`,
> `τ = 220 ms`) smooths these transitions by introducing a ramp-up and
> ramp-down of the control voltage.

The filtered signal is then buffered by a `MCP6021` voltage follower (powered at `+2.5 V / -2.5 V`) to prevent the `RC` filter from interacting with the downstream resistors of the Howland stage, which could distort the output current.

## Wien Oscillator

The Wien oscillator generates a stable `300 mVp` sine wave at `1.5 Hz`, powered by the `±9 V` rails.

> ### What is a Wien oscillator?
> A Wien oscillator is an analog sine wave generator built around an op-amp
> with a frequency-selective RC feedback network. It is one of the simplest circuits
> capable of producing a clean, low-distortion sine wave at a precise frequency, set by
> the RC components: `f = 1 / (2π × R × C)`
>
> It was chosen here for its simplicity (very few components), its low frequency capability,
> and the quality of its sine wave output, making it well suited for a `1.5 Hz` stimulation signal.

> ### Why add a sine wave to the control signal?
> The vestibular system *may* be more sensitive to **varying stimuli** than to a
> static DC signal. According to literature, the brain tends to adapt and progressively ignore a
> constant input. Adding a low-frequency sine wave keeps the vestibular nerve
> continuously activated, enhancing the perceived effect of the stimulation.

The oscillator output is summed with the joystick control signal in the next stage. At `0 V` joystick input (center position), the sine wave alone produces a small symmetric oscillation around `0 mA`, resulting in no net stimulation. The DC bias set by the joystick shifts this oscillation toward positive or negative currents, inducing a directional tilt sensation.
