# Discrete TIA Board Documentation

## Introduction
This repository provides a comprehensive overview of the design and testing of a Discrete TIA board. The board is designed to perform critical functions, such as transimpedance amplification (TIA), voltage amplification (2VB Generator), and low-pass filtering. The primary goal of this documentation is to present the board’s specifications, design details, and test results, along with addressing potential issues and troubleshooting methods encountered during the development process.

## Board Overview
### Specifications
The following are the specifications of the board.
- TIA Gain of 110k.
- Low pass filter with bandwidth of 5kHz.
- 8 Independent TIA channels.
- Power Supply voltage of 5V

### Components List
The components used are as follows,
- Opamps
  - ADA4610-2
  - AD8510
- Voltage Regulators
  - LTC1983ES6-5
  - LTC1751EMS8-5
- Miscellaneous
  - PJ-002AH-SMT-TR
  - TFM-103-01-L-S
  - MSS-102545-28A-D

## Test Results
### Basic Functionality Test
This test was intended to check for design, manufacturing, or assembly problems. This test is divided into three parts, testing the TIA, the voltage amplifier and the low pass filter.

### Noise Test
To measure the noise, the board inputs were left open or connected to the NI DAQ outputs while generating a signal of 0 VDC (Gnd). The output voltages were acquired for approximately 2 seconds.
Then, the signal observed at the output was squared, and the Fast Fourier Transform (FFT) was applied to obtain the output voltage noise Power Spectral Density (PSD). Finally, the Root Mean Square (RMS) value of the PSD was calculated by integrating over a frequency range from 0 to 5 kHz. This will give us the output referred noise. To get the input referred current noise we can divide it by the gain of the TIA (110 × 10^3). These steps were performed using Matlab, and the code was initially developed for another project with the same purpose.

## Trouble Shooting
### High Frequency Noise from the Power Supply
The circuit’s output was found to be affected by noise present in the power supply. Further analysis identified the +5V voltage regulator as the source of this noise, leading to fluctuations in the output and impacting the circuit’s performance. To resolve this issue, additional filtering or decoupling may be necessary to reduce the noise and ensure stable circuit operation.

Moreover, it was discovered that the unregulated voltage from the power supply remained stable without significant noise. As a temporary solution to address the issue, the +5V voltage regulator was removed from the board, and the channels were directly supplied with power from the power supply.

### Noise Induced by the Cables
Significant noise was observed due to the contribution from the input cables used in the circuit. To mitigate this issue, a solution was implemented by shielding the input cables with ground. Figure 11 illustrates the shielding process employed to eliminate noise induced by the input cables.

The shielding technique involves surrounding the input cables with a conductive material, such as a grounded metal shield. This shield acts as a barrier to external electromagnetic interference and helps prevent noise from coupling into the cables. By connecting the shield to ground, any induced noise or electromagnetic interference is redirected away from the sensitive signal-carrying conductors, reducing the overall noise level in the circuit.

With this shielding in place, the circuit should experience reduced noise from external sources, leading to improved signal integrity and more accurate measurements or performance. However, it is essential to ensure proper grounding and shielding techniques are used to achieve optimal noise reduction benefits.

### Noise induced by the Power Adapter
While conducting measurements for the noise analysis, it was observed that the power adapter itself produced a low-frequency noise. This noise likely originated from the power adapter’s internal components and circuitry, introducing unwanted fluctuations into the power supply.

To address this issue and achieve better noise performance, power bank was utilized. Power banks are known to provide a more stable and cleaner power output compared to some power adapters, especially when it comes to low-frequency noise.

By using the power bank as the power source, the low-frequency noise interference was reduced. As a result, the measurements obtained were more reliable and accurate, with less noise affecting the overall performance of the circuit.

Choosing an appropriate and reliable power source is crucial in noise-sensitive applications, as the quality of the power supply can directly impact the circuit’s performance and measurement accuracy. The switch to a power bank in this case demonstrates the importance of considering the power supply’s noise characteristics to ensure optimal functioning of the circuit and precise measurements.
