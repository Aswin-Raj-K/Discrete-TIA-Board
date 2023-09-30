# Discrete TIA Board Documentation
## NYU NANOELECTRONICS LAB
**Note:** Zoom in to see the plots clearly

### Contents
1. [Introduction](#introduction)
2. [Board Overview](#board-overview)
   1. [Specifications](#specifications)
   2. [Design](#design)
      - [TIA](#tia)
      - [2VB Generator (Voltage Amplifier)](#2vb-generator-voltage-amplifier)
      - [Low Pass filter](#low-pass-filter)
   3. [TIA Channel](#tia-channel)
   4. [Components List](#components-list)
3. [Test Results](#test-results)
   1. [Basic Functionality Test](#basic-functionality-test)
      - [Testing TIA](#testing-tia)
      - [Testing the voltage amplifier](#testing-the-voltage-amplifier)
      - [Testing the Low Pass Filter](#testing-the-low-pass-filter)
   2. [Noise Test](#noise-test)
4. [Trouble Shooting](#trouble-shooting)
   1. [High Frequency Noise from the Power Supply](#high-frequency-noise-from-the-power-supply)
   2. [Noise Induced by the Cables](#noise-induced-by-the-cables)
   3. [Noise induced by the Power Adapter](#noise-induced-by-the-power-adapter)

## Introduction
This documentation provides a comprehensive overview of the design and testing of a Discrete TIA board. The board is designed to perform critical functions, such as transimpedance amplification (TIA), voltage amplification (2VB Generator), and low-pass filtering. The primary goal of this documentation is to present the board’s specifications, design details, and test results, along with addressing potential issues and troubleshooting methods encountered during the development process.

## Board Overview
### Specifications
The following are the specifications of the board.
- TIA Gain of 110k.
- Low pass filter with bandwidth of 5kHz.
- 8 Independent TIA channels.
- Power Supply voltage of 5V

### Design
This section goes through the design procedure followed for the development of the board.
#### TIA
Figure 1 shows the TIA. It can be easily derived the relation between input and output. which is given by the equation, if RF = R,
Vout = −IIN RF
where RF is the feedback resistor and CF is the feedback capacitor. The value of RF is chosen as 100kΩ to achieve a TIA gain of 110 × 103. The value of CF is chosen to be 10pF based on the datasheet of the opamp. It was later verified through simulation in Orcad Capture to ensure a stable output.

#### 2VB Generator (Voltage Amplifier)
To eliminate the contribution of VB in the output of the TIA, we need to apply 2VB at the non-inverting terminal of the TIA. To generate this 2VB signal from VB, a simple non-inverting amplifier with a gain of 2 is used. The amplifier is depicted in figure 2. The output of the amplifier is given by the equation:
Vout = (1 + R2/R1) * Vin
To achieve a gain of 2, the values of R1 and R2 are set to be equal and chosen as 110kΩ.

#### Low Pass filter
The low-pass filter consists of a single resistor and capacitor, as depicted in figure 3. The cut-off frequency (fc) of the filter is determined by the equation:
fc = 1 / (2πRLCL)
To achieve a cut-off frequency of 5kHz, RL is chosen as 100kΩ and CL is chosen as 300pF.

### TIA Channel
A single TIA independent channel is as shown in figure 4.

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
#### Testing TIA
The TIAs were used as inverting amplifiers, and a sinusoidal signal of 1V at 1 kHz was applied at the VB EXT, IN is left unconnected and VB is grounded. The connections as summarized in the table 2

#### Testing the voltage amplifier
This test is intended to check the working of the voltage amplifier. The amplifier is designed to have a non-inverting gain of 2. To verify this, a sinusoidal signal of 1kHz is supplied to the input VB, and the output is observed at pin 2 of the switch (pin facing into the center of the board).

#### Testing the Low Pass Filter
This test is intended to check the functionality of the Low Pass Filter. The connections are the same as those in table 2. Here, we will send an input of a sinusoid with a frequency of 6kHz to the VB EXT.

### Noise Test
To measure the noise, the board inputs were left open or connected to the NI DAQ outputs while generating a signal of 0 VDC (Gnd). The output voltages were acquired for approximately 2 seconds.
Then, the signal observed at the output was squared, and the Fast Fourier Transform (FFT) was applied to obtain the output voltage noise Power Spectral Density (PSD). Finally, the Root Mean Square (RMS) value of the PSD was calculated by integrating over a frequency range from 0 to 5 kHz. This will give us the output referred noise. To get the input referred current noise we can divide it by the gain of the TIA (110 × 103). These steps were performed using Matlab, and the code was initially developed for another project with the same purpose.

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
