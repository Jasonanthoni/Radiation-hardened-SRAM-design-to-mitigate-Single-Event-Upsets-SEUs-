# Radiation Hardened SRAM Design to Mitigate Single Event Upsets (SEUs)

> Design, simulation, and comparative analysis of conventional 6T SRAM and radiation-hardened DICE SRAM for mitigating Single Event Upsets (SEUs) using **Cadence Virtuoso (180 nm CMOS Technology)**.

---

## Overview

Static Random Access Memory (SRAM) is one of the most widely used memory architectures in modern processors due to its high speed and low latency. As CMOS technology continues to scale, SRAM cells become increasingly susceptible to **Single Event Upsets (SEUs)** caused by energetic particles such as cosmic rays, neutrons, and alpha particles.

This project investigates the vulnerability of a conventional **6T SRAM** cell to radiation-induced soft errors and proposes a **Dual Interlocked Storage Cell (DICE)** architecture as a Radiation Hardened by Design (RHBD) solution.

The work was implemented and simulated using **Cadence Virtuoso** in **180 nm CMOS technology**. The project includes functional verification, transient current injection for SEU analysis, critical charge estimation, and a comparative study between conventional 6T SRAM and DICE SRAM.

---

## Table of Contents

- Background and Methodology
- Conventional 6T SRAM
- DICE SRAM
- Results
- Contributions
- Repository Structure

---

# Background and Methodology

## What is a Single Event Upset (SEU)?

A **Single Event Upset (SEU)** is a radiation-induced soft error that occurs when an energetic particle strikes a sensitive node inside a semiconductor device. The particle generates electron-hole pairs within the silicon substrate, resulting in a transient current pulse. If the collected charge exceeds the **critical charge (Q<sub>crit</sub>)**, the stored logic value changes, producing a bit flip.

Unlike permanent hardware failures, SEUs do not physically damage the device. However, they can lead to incorrect computations and unreliable operation in safety-critical systems such as satellites, spacecraft, medical electronics, automotive controllers, and defense applications.

---

## Design Methodology

The complete project was carried out in the following sequence:

- Design of a conventional 6T SRAM cell
   - Design of the Precharge Circuit
- Functional verification through Read, Write, and Hold operations
- Radiation fault injection using a double exponential current pulse
- Determination of the threshold current and critical charge
- Design and implementation of the DICE SRAM architecture
   - Design of the Precharge Circuit
- Functional verification of the DICE SRAM
- Radiation fault injection and recovery analysis
- Comparative study of both architectures

---

# Conventional 6T SRAM

## Schematic

<img width="1732" height="881" alt="white_sram" src="https://github.com/user-attachments/assets/55fa7c9b-2bf1-4e3b-9d56-35b7a26f378d" />


The conventional SRAM cell consists of six MOS transistors arranged as two cross-coupled CMOS inverters and two access transistors. The cross-coupled inverters provide two stable states representing logic '0' and logic '1', while the access transistors connect the storage nodes to the complementary bit lines during read and write operations.

---

## Precharge Circuit

*Insert Precharge Circuit Image*

Before every read operation, both bit lines are precharged to the supply voltage. The precharge circuit ensures identical initial conditions on both bit lines, allowing the sense amplifier to accurately detect the stored data.

Without precharging, residual voltages from previous operations could introduce incorrect sensing and increase read delay.

---

## Transistor Sizing

One of the most important aspects of SRAM design is transistor sizing. Proper transistor sizing determines the stability of the memory cell during read, write, and hold operations.

The SRAM cell contains three different transistor groups:

- Pull-Up PMOS
- Pull-Down NMOS
- Access NMOS

The pull-down NMOS transistors are generally made stronger than the access transistors to prevent accidental bit flips during read operations. Similarly, the access transistors must be sufficiently strong to successfully overwrite the stored data during write operations.

Two important design parameters are:

### Cell Ratio (CR)

\[CR=\frac{W_{PullDown}}{W_{Access}}\]

A larger Cell Ratio improves read stability by ensuring that the pull-down transistor can maintain the stored logic level while the bit line attempts to discharge the storage node.

### Pull-Up Ratio (PR)

\[
PR=\frac{W_{Access}}{W_{PullUp}}
\]

The Pull-Up Ratio determines the write ability of the SRAM cell. Proper transistor sizing provides an optimal balance between read stability and write performance.

### Transistor Dimensions Used

| Transistor | Width | Length |
|------------|-------|--------|
| Pull-Up PMOS | *(Your Value)* | *(Your Value)* |
| Pull-Down NMOS | *(Your Value)* | *(Your Value)* |
| Access NMOS | *(Your Value)* | *(Your Value)* |

The transistor dimensions were selected after several iterations to achieve stable read, write, and hold operations while maintaining adequate noise margins.

---

## Write, Hold and Read Operations (Functional Verification)

*Insert combined waveform image*

Transient simulations were performed to verify the functionality of the SRAM cell.

### Write Operation

The desired data was applied to the complementary bit lines while enabling the Word Line. The simulations confirmed successful writing of both logic '0' and logic '1'.

### Read Operation

Both bit lines were precharged before enabling the Word Line. The stored data was successfully read without disturbing the internal storage nodes.

### Hold Operation

During the hold state, the Word Line remained LOW and the cross-coupled inverters maintained the stored data without external intervention.

The successful completion of these simulations confirmed the correct implementation of the conventional SRAM cell.

---

## Charge Injection

*Insert current pulse waveform*

Radiation effects were emulated using a double exponential transient current pulse injected into the sensitive storage node.

The injected current is expressed as

\[
I(t)=I_{peak}\left(e^{-t/\tau_{fall}}-e^{-t/\tau_{rise}}\right)
\]

The injected current was gradually increased until the stored logic state flipped, allowing the critical charge of the SRAM cell to be determined.

---

## Explanation of SEU

When the injected charge exceeded the critical charge of the SRAM cell, the regenerative feedback of the cross-coupled inverters forced the memory into the opposite stable state.

This resulted in a permanent bit flip, confirming the susceptibility of conventional SRAM cells to radiation-induced Single Event Upsets.

---

# DICE SRAM

## Schematic

<img width="756" height="591" alt="Screenshot 2026-07-09 231851" src="https://github.com/user-attachments/assets/55263ff8-401c-4484-8ee0-ee5ce94b4950" />


The Dual Interlocked Storage Cell (DICE) architecture improves radiation tolerance by storing information across four interlocked storage nodes instead of two. The redundant storage structure allows the memory cell to recover automatically after transient disturbances.

---
## DICE cell with the Precharge Circuit for the DICE Cell

<img width="1731" height="880" alt="DICE_with_PCC" src="https://github.com/user-attachments/assets/9d2ec3c4-f8e1-4d0e-9639-c056ec6e6c06" />

Before every read operation, both bit lines are precharged to the supply voltage. The precharge circuit ensures identical initial conditions on both bit lines, allowing the sense amplifier to accurately detect the stored data.

Without precharging, residual voltages from previous operations could introduce incorrect sensing and increase read delay.

---

## Write, Hold and Read Operations (Functional Verification)

*Insert combined waveform*

The DICE SRAM successfully performed normal read, write, and hold operations under identical simulation conditions as the conventional SRAM.

The functional behavior remained identical while providing improved robustness against radiation-induced disturbances.

---

## Charge Injection

The same transient current pulse used for the conventional SRAM analysis was injected into one of the sensitive storage nodes of the DICE SRAM.

Although the struck node experienced a temporary voltage disturbance, the remaining storage nodes preserved the correct logic value throughout the simulation.

---

## Explanation of SEU

Unlike the conventional SRAM, the DICE architecture automatically restored the disturbed node after the transient current pulse disappeared.

The redundant storage nodes continuously reinforced the original logic value, preventing a permanent bit flip.

This self-recovery mechanism significantly improves radiation tolerance and makes the DICE architecture suitable for safety-critical applications.

---

# Results

The simulation results demonstrate the effectiveness of the DICE architecture in mitigating Single Event Upsets.

Both memory cells successfully completed read, write, and hold operations under normal operating conditions. However, under transient current injection, the conventional 6T SRAM experienced a permanent bit flip once the injected charge exceeded its critical threshold. In contrast, the DICE SRAM automatically recovered after the transient disturbance without requiring any external correction mechanism.

These results confirm that architectural redundancy significantly improves SRAM reliability in radiation-prone environments.

---

# Contributions

- Designed a conventional 6T SRAM using Cadence Virtuoso.
- Implemented a precharge circuit for reliable read operations.
- Verified read, write, and hold functionality through transient simulations.
- Simulated radiation-induced Single Event Upsets using transient current injection.
- Determined the threshold current and critical charge of the conventional SRAM.
- Designed and implemented a radiation-hardened DICE SRAM architecture.
- Demonstrated automatic recovery from radiation-induced transient disturbances.
- Performed a comparative analysis between conventional and radiation-hardened SRAM architectures.

---

# Repository Structure

X
X
X
X
X
X

---

## Tools Used

- Cadence Virtuoso
    - 180 nm CMOS Technology
---

## Author

**Jason Anthoni**
