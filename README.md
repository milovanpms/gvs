<img align="left" style="width:150px" src="./assets/human.gif" width="170px">

**Controlling a human being with a game controller: it seems crazy, right? You're not dreaming: just two electrodes and a small device can help you take control of the balance of your worst enemies!**

The ***G**énialissime **V**ecteur de **S**ensations (**GVS**)* is a **Galvanic Vestibular Stimulation** device built with analog electronics, designed to induce balance and orientation sensations through controlled current stimulation. 

<p align="center">
  <img src="https://img.shields.io/badge/status-untested-orange"/>
  <img src="https://img.shields.io/badge/hardware-analog-lightgrey"/>
  <img src="https://img.shields.io/badge/simulation-LTspice-8A2BE2"/>
</p>

<h1 align="center">Génialissime Vecteur de Sensations</h1>

<h5 align="center"><a href="https://milovan.me">milovan.me</a></h5>

> [!CAUTION]
> This device can deliver **electrical current** to **a human skull**. By using this project, you agree that you are **solely responsible** for any accident, dizziness, nausea, or existential crises that may occur. I'm **not** a doctor: do not use it if you don't know what you're building and doing. Max current is hardware-limited to 5mA, but that's still enough to ruin your afternoon.
> 
> **Build and use it at your own risk.**

## 🧠 What is GVS?

> #### Check out the [detailed documentation](./docs/gvs.md).

**G**alvanic **V**estibular **S**timulation (**GVS**) is the process of sending electric messages (*in this project, a low-level DC current*) to a nerve in **the vestibular system**, located in the ear, that maintains balance. We "access" the vestibular system through **the mastoid process**, a conical projection forming a bony prominence behind and below the ear.

### Vestibular system

The vestibular system is a sensory organ, constitutive of the inner ear, that creates **the sense of balance and spatial orientation** for the function of coordinating movement with balance. This organ is present in most mammals.

Troubles of the vestibular system can lead to **dizziness**: this device exploits this by applying **a controlled DC current to the mastoid process**, artificially triggering the vestibular nerve and **inducing balance and orientation sensations**.

## Documentation
### Find your happiness:
- 🧠 [What is GVS?](./docs/gvs.md): Vestibular system, physiological effects and stimulation principles
- ⚙️ [Circuit Design](./docs/circuit.md): Improved Howland current source, push-pull follower and component selection
- 🔒 [Safety](./docs/safety.md): Current limits, galvanic isolation, emergency switch and build protocol
- 🧪 [Simulation](./docs/simulation.md): LTspice models, transient analysis and scenarios

## Repository Structure
```
├── spice/           # SPICE schematic and simulation files
├── docs/            # Documentation, conception notes
└── assets/          # Images, schematics and illustrations
```
