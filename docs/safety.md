# 🔒 Safety

> [!CAUTION]
> This device can deliver **electrical current** to **a human skull**. By using this project, you agree that you are **solely responsible** for any accident, dizziness, nausea, or existential crises that may occur. I'm **not** a doctor: do not use it if you don't know what you're building and doing. Max current is hardware-limited to 5mA, but that's still enough to ruin your afternoon.
> 
> **Build and use it at your own risk.**

## Critical Limits

| Parameter | Value |
|-----------|-------|
| Maximum current | `5 mA`, `3.3 mA` recommended |
| Power supply | `2× 9 V` batteries (no mains connection) |
| Isolation | Full galvanic isolation from any mains-powered equipment |

> ### Why battery power only?
> 
> Powering the device exclusively from batteries eliminates any electrical
> path to the mains supply, removing the risk of a fault condition delivering
> dangerous current through the electrodes via a grounded connection.

## Hardware Protections

The circuit includes three layers of protection between the signal source and the electrodes (see [Circuit Design documentation](./circuit.md) for full details):

1. **Current sense + transistor limiter** (`BD139` / `BD140`): Monitors the output current via `Rsense` and clamps the Howland op-amp input when the threshold is exceeded.
2. **Series protection resistor** (`Rserie`): A hard current limit as a *last chance* safeguard, independent of the active limiter.
3. **Emergency stop button**: Instantly forces the Howland input to `0 V`, dropping the output current to `0 mA` without cutting the circuit power.

> [!WARNING]
> As detailed in the [Circuit Design documentation](./circuit.md), the `Rserie` protection in
> its current form has limited effectiveness and a **DC blocking capacitor**
> should be considered as a future improvement to guard against DC faults.

## Electrode Safety

- **TEST YOUR CIRCUIT BEFORE CONNECTING IT TO A HUMAN!** Use resistive loads (`~1-3 kΩ`) in place of electrodes, and verify the output current with a multimeter or oscilloscope before any human use.
- Consider using **Ag/AgCl self-adhesive electrodes**
- Consider using **medical conductive gel** between the skin and electrodes to reduce contact impedance and minimize irritation.
- **Clean the skin with alcohol** before electrode placement to remove oils and improve contact quality.
- Place electrodes precisely on the **mastoid processes**, behind each ear (see [What is GVS?](./gvs.md)).

## Build Recommendations

1. **Non-conductive enclosure**: The device housing must be fully non-conductive to prevent any accidental contact with the circuit.
2. **Galvanic isolation**: Keep the control electronics (mainly the joystick interface) isolated from the output stage.
3. **TEST YOUR CIRCUIT BEFORE CONNECTING IT TO A HUMAN! (please)** Use resistive loads (`~1-3 kΩ`) in place of electrodes, and verify the output current with a multimeter or oscilloscope before any human use.
4. **Calibration**: Confirm actual output current matches expected values across the full range of the joystick before first use.
5. **Accessible emergency stop**: The emergency stop button must be easily reachable by the user at all times during operation.
6. **Never use it alone**: Always have a second person present during testing or use, able to hit the emergency stop or intervene if needed. *Plus on est de fous, plus on rit !*
