# 534_Final_Project
534 Final Project Two-Stage Programmable Gain OTA

# Introduction
For the final project of ECE 534, I was asked to create a high-precision, programmable-gain, multi-stage, switched-capacitor amplifier. I created this circuit by using multiple techniques to account for non-idealities encountered in practice. By using a two-phase non-overlapping clock, I created two phases in which the circuit operates in different regions: pre-amplification and amplification. This allows for switched-capacitor amplification.
One of the issues I had to address was using MOSFETs as resistors. MOSFETs have a non-constant (R_{on}) value that can drastically affect circuit functionality. To mitigate this, I used constant (V_{GS}) sampling to achieve a more consistent (R_{on}). Constant (V_{GS}) sampling employs a clock multiplier circuit to generate an output of (2V_{DD}). Then, using a bootstrap capacitor, an inverter, and proper biasing, I created a switch with a more constant (R_{on}) value.
I also could not use ideal switches, so I implemented CMOS configurations to create the switches. However, CMOS switches introduce charge injection. To eliminate this effect, I used bottom-plate sampling, which employs a slightly delayed clock to redirect any residual charge injection to ground. This grounding does not affect the overall functionality of the switched-capacitor amplifier.
Finally, I created a programmable gain selector to allow for gain adjustment. This gain selection allows the input signal to be amplified by factors of 4, 8, or 12, which is essential for the multi-stage output gain.

