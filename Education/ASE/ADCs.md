---
tags:
  - "#ASE"
  - "#ADC"
---
### General aspects
#### ADC

An **ADC (Analog-to-Digital Converter)** converts analog signals (continuous) into digital values (discrete) for processing by a microcontroller or processor.
#### DAC

A **DAC (Digital-to-Analog Converter)** converts digital values into analog signals, allowing microcontrollers to generate continuous signals for audio, motor control, or other analog applications.
#### Continuous Signal

A **continuous signal** is an analog signal that varies smoothly over time without discrete steps, meaning it can have infinite values within a given range. 
#### Discrete Signal

A **discrete signal** is a signal that has distinct, separate values at specific time intervals, rather than varying continuously. It is typically represented as a sequence of numbers, like digital data in a computer.

![[Pasted image 20250331094249.png|739x370]]
#### Sampling Process in AD conversion

The **sampling process** in ADC is the process of measuring an analog signal at regular time intervals to create a discrete representation. The sampling rate, measured in **samples per second (Hz)**, determines how often the signal is measured. A higher sampling rate captures more detail, following the **Nyquist theorem**, which states that the sampling rate must be at least **twice the highest frequency** of the signal to avoid distortion (aliasing).
#### Quantization Process in AD conversion

**Quantization** in ADC is the process of mapping sampled analog values to the closest available digital value based on the ADC’s resolution. Since digital systems have a limited number of discrete levels, quantization introduces **quantization error**, which is the <u>difference between the actual analog value and the assigned digital value</u>. Higher-bit ADCs reduce this error by providing more precise digital representations.
#### Reconstruction Process in DA conversion

The **reconstruction process** in DAC converts discrete digital values back into a continuous analog signal. This typically involves **holding each digital value** for a short time (zero-order hold) and smoothing the output using a **low-pass filter** to remove unwanted high-frequency artifacts. The goal is to approximate the original analog signal as closely as possible.
#### The sampling rate of ADCs/DACs

The **sampling rate** of ADCs/DACs is the number of samples taken or output per second, measured in **samples per second (SPS) or hertz (Hz)**.
- **ADC Sampling Rate:** Determines how often an analog signal is measured. Must be at least **twice the highest frequency** of the signal (Nyquist theorem) to prevent aliasing.
- **DAC Sampling Rate:** Defines how often digital values are converted to analog. A higher rate provides smoother output but requires more processing power.
#### Bit Resolution of ADCs/DACs

The **bit resolution** of ADCs/DACs defines the number of discrete levels they can represent. It determines the precision of conversion.
- **ADC Resolution:** The number of bits used to represent an analog input. A **12-bit ADC** divides the input range into **2^{12}=4096** levels. Higher resolution means lower quantization error.
- **DAC Resolution:** The number of bits used to generate an analog output. A **16-bit DAC** produces finer voltage steps than an **8-bit DAC**, resulting in smoother signals.
#### Nyquist Frequency & aliasing

The **Nyquist frequency** is **half of the sampling rate** of an ADC. It represents the highest frequency that can be accurately captured without distortion. According to the **Nyquist theorem**, the sampling rate must be at least **twice the highest frequency** of the signal to prevent errors.
**Aliasing** occurs when a signal is sampled at a rate lower than twice its highest frequency. This causes high-frequency components to appear as lower frequencies in the digital output, leading to incorrect signal representation and distortion. To prevent aliasing, systems use **anti-aliasing filters** to remove high-frequency content before sampling.
#### Reference Voltage of ADCs & DACs

The **reference voltage (Vref)** of ADCs and DACs defines the maximum voltage they can measure (ADC) or output (DAC):
- **ADC Vref:** Determines the input voltage range, for example, with **Vref = 3.3V** and a **12-bit ADC**, the smallest measurable step is **3.3V / 4096 ≈ 0.8 mV**.
- **DAC Vref:** Sets the output range, if **Vref = 5V**, a **10-bit DAC** can generate voltages in **5V / 1024 ≈ 4.88 mV** steps.
A stable Vref improves accuracy and reduces noise in conversions.
#### Successive Approximation Register (SAR) ADC

A **Successive Approximation Register (SAR) ADC** converts an analog signal into a digital value by using a **binary search algorithm**. It works as follows:
1. The ADC **samples** the input voltage and holds it,
2. The **SAR starts with the most significant bit (MSB)** and sets it to **1**, comparing the resulting digital output to the input voltage using a **DAC and comparator**,
3. If the DAC output is **higher** than the input voltage, the bit is cleared to **0**, otherwise, it remains **1**,
4. The process repeats for the **next lower bit** until all bits are determined.
This method is **fast and power-efficient**, making SAR ADCs popular in microcontrollers and precision measurement applications.




# Potentiometer Calculations

![[image.png]]

with a typical use of 1mA for the LM335 and with a constant flow of 3.3V.

At 25ºC outputs a average typical voltage needed of 2.98V

3.3V - 2.98V = 0.32V

$$
R = \frac{V}{I} \iff R = \frac{0.32 \, \text{V}}{1 \, \text{mA}} = \frac{0.32 \, \text{V}}{1 \times 10^{-3} \, \text{A}} = 320 \, \Omega
$$
