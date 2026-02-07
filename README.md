# 534_Final_Project

534 Final Project Two-Stage Programmable Gain OTA
Note: Not all pictures included
# Introduction

For the final project of ECE 534, I was asked to create a high-precision, programmable-gain, multi-stage, switched-capacitor amplifier. I created this circuit by using multiple techniques to account for non-idealities encountered in practice. Using a two-phase, non-overlapping clock, I created two phases in which the circuit operates in different regions: pre-amplification and amplification. This allows for switched-capacitor amplification.

One issue I had to address was using MOSFETs as resistors. MOSFETs have a non-constant (R_{on}) value that can drastically affect circuit functionality. To mitigate this, I used constant (V_{GS}) sampling to achieve a more consistent (R_{on}). Constant (V_{GS}) sampling employs a clock multiplier circuit to generate an output of (2V_{DD}). Then, using a bootstrap capacitor, an inverter, and proper biasing, I created a switch with a more constant (R_{on}) value.

I also could not use ideal switches, so I implemented CMOS configurations to create the switches. However, CMOS switches introduce charge injection. To eliminate this effect, I used bottom-plate sampling, which employs a slightly delayed clock to redirect any residual charge injection to ground. This grounding does not affect the overall functionality of the switched-capacitor amplifier.

Finally, I created a programmable gain selector to allow for gain adjustment. This gain selection allows the input signal to be amplified by factors of 4, 8, or 12, which is essential for the multi-stage output gain.


# Simulation Results
## Ideal Simulation Results
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure2_3.png" alt="Gain Times Four and Eight">
        <figcaption>Figure 2 & 3: Gain of Times Four and Eight (Capacitor Ratios)</figcaption>
</figure>

<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure4.png" alt="Gain of Times Twelve">
        <figcaption>Figure 4: Gain of Times Twelve (C1 = 24p, C2 = 2p)</figcaption>
    </figure>

## Ideal Results Discussion:
After building and testing the ideal switched capacitor amplifier, we can see the desired outputs of gain amplification, with our input magnitude being 10uV. The table below shows that our maximum gain error for all cases is lower than the specified 1%. 

# Intermediate Simulation Results
## Two-Phase Non-Overlapping Clock
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure5_6.png" alt="Non-Overlapping Clock">
        <figcaption>Figure 5 & 6: Two-Phase Non-Overlapping Clock Schematic and Waveform</figcaption>
</figure>
## Two-Phase Non-Overlapping With Delay Discussion:
To create the two-phase non-overlapping with delay, we utilized nor gates and inverters to create the two-phase overlap. Then, using a series of inverters and a capacitor, we created the delays. As seen in Figure 7, the two clock phases do not overlap and have independent delays. 

# Constant Vgs Sampling
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure8.png" alt="Constant Vgs Sampling Schematic">
        <figcaption>Figure 8: Constant Vgs Sampling Schematic</figcaption>
</figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure9.png" alt="Testing Circuit Simulation">
        <figcaption>Figure 9: Constant Vgs Sampling Testing Circuit (Individual)</figcaption>
    </figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure10.png" alt="Ideal Circuit Implementation">
        <figcaption>Figure 10: Constant Vgs Sampling Implemented into Ideal Circuit</figcaption>
    </figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure11.png" alt="Simulation Results">
        <figcaption>Figure 11: Constant Vgs Sampling Ideal Circuit Simulation Results</figcaption>
    </figure>
## Implementation of Constant Vgs Sampling Discussion:
	By implementing the same schematic (Figure 8) we used in class, we can create a switch independent of the NMOS switch's Ron resistance. We see that the positive portion of the Vgs sampling is a perfect sample of the input with slight noise and variation. However, there was a larger problem with the negative sampling portion (Figure 9). While I could not resolve this problem, the issue is most likely due to gate sizes and charge injection in MOSFET switches.
After testing, we implemented the switch configuration in the switched-capacitor circuit (Figure 10). When implementing, I had to change the values of M8 and M9 by performing a parametric sweep to achieve the best results with minimum settling-time error and the spikes seen in Figure 11. 
These spikes were assumed to be issues with the ideality of the surrounding switches. With ideal switches, sudden closes and opens create a cadence that doesn’t know what to do with, so it results in a spike. I was able to reduce some of the spikes by replacing the ideal switches with CMOS switches.
# CMOS Switch
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure12_13.png" alt="CMOS Switch">
        <figcaption>Figure 12 & 13: CMOS Switch Schematic and Test Bench Simulation</figcaption>
    </figure>
## CMOS Discussion:
The CMOS switch contains a PMOS and an NMOS, both parallel to a clock signal that controls each gate. We can test the CMOS switch's operational functionality by generating a clock signal and an input signal, with a capacitor on the output. The output should be a sample and hold, as seen in Figure 13. 
By replacing the ideal switches in our switched capacitor circuit, we could dampen the effect of the ideal abnormality spikes. This also gives us a more accurate representation of how to implement this circuit in hardware. 
I also added the ability to change the widths and lengths of the PMOS and NMOS in the circuit's symbol equivalent. This allowed me to change each switch individually to achieve the desired gain and utilize bottom plate sampling correctly. 

# Bottom Plate Sampling
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure14.png" alt="Bottom Plate Sampling">
        <figcaption>Figure 14: Bottom Plate Sampling Circuit Configuration (Zoomed)</figcaption>
</figure>
Bottom plate sampling allows us to take any charge injection that happens from the constant Vgs sampling switch and ground it. Without the bottom plate sampling switch, when our input changes constantly, our charge injection varies, which can affect the output of the next stage. This takes away any chance of the charge injection being amplified. 

# Single Stage Final Results
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure17.png" alt="Non-Ideal Amplifier">
        <figcaption>Figure 17: Non-Ideal Single-Stage Switch Capacitor Amplifier</figcaption>
</figure>
Gain Amplification,Actual,Expected,Maximum Gain Error
4,936.95,940,0.32%
8,971.08,980,0.91%
12,1.0154,1.02,0.45%
