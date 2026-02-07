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
</figure>

<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure4.png" alt="Gain of Times Twelve">
    </figure>

## Ideal Results Discussion:
After building and testing the ideal switched capacitor amplifier, we can see the desired outputs of gain amplification, with our input magnitude being 10uV. The table below shows that our maximum gain error for all cases is lower than the specified 1%. 

# Intermediate Simulation Results
## Two-Phase Non-Overlapping Clock
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure5_6.png" alt="Non-Overlapping Clock">
</figure>
## Two-Phase Non-Overlapping With Delay Discussion:
To create the two-phase non-overlapping with delay, we utilized nor gates and inverters to create the two-phase overlap. Then, using a series of inverters and a capacitor, we created the delays. As seen in Figure 7, the two clock phases do not overlap and have independent delays. 

# Constant Vgs Sampling
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure8.png" alt="Constant Vgs Sampling Schematic">
</figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure9.png" alt="Testing Circuit Simulation">
    </figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure10.png" alt="Ideal Circuit Implementation">
    </figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure11.png" alt="Simulation Results">
    </figure>
## Implementation of Constant Vgs Sampling Discussion:
By implementing the same schematic (Figure 8) we used in class, we can create a switch independent of the NMOS switch's Ron resistance. We see that the positive portion of the Vgs sampling is a perfect sample of the input with slight noise and variation. However, there was a larger problem with the negative sampling portion (Figure 9). While I could not resolve this problem, the issue is most likely due to gate sizes and charge injection in MOSFET switches.

After testing, I implemented the switch configuration in the switched-capacitor circuit (Figure 10). When implementing, I had to adjust the values of M8 and M9 by performing a parametric sweep to achieve the best results, minimizing settling-time error and the spikes shown in Figure 11. 

These spikes were assumed to be issues with the ideality of the surrounding switches. With ideal switches, sudden closes and opens create a cadence that doesn’t know what to do with, so it results in a spike. I reduced some of the spikes by replacing the ideal switches with CMOS switches.
# CMOS Switch
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure12_13.png" alt="CMOS Switch">
    </figure>
## CMOS Discussion:
The CMOS switch contains a PMOS and an NMOS, both parallel to a clock signal that controls each gate. We can test the CMOS switch's operational functionality by generating a clock signal and an input signal, with a capacitor on the output. The output should be a sample and hold, as seen in Figure 13. 

By replacing the ideal switches in our switched-capacitor circuit, we can dampen the effects of ideal abnormality spikes. This also gives us a more accurate representation of how to implement this circuit in hardware. 

I also added the ability to change the widths and lengths of the PMOS and NMOS in the circuit's symbol equivalent. This allowed me to change each switch individually to achieve the desired gain and utilize bottom plate sampling correctly. 

# Bottom Plate Sampling
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure14.png" alt="Bottom Plate Sampling">
</figure>
Bottom plate sampling allows us to take any charge injection that happens from the constant Vgs sampling switch and ground it. Without the bottom plate sampling switch, when our input changes constantly, our charge injection varies, which can affect the output of the next stage. This takes away any chance of the charge injection being amplified. 

# Single Stage Final Results
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure17.png" alt="Non-Ideal Amplifier">
</figure>

| Gain Amplification | Actual  | Expected | Maximum Gain Error |
|--------------------|---------|----------|--------------------|
| 4                  | 936.95  | 940      | 0.32%              |
| 8                  | 971.08  | 980      | 0.91%              |
| 12                 | 1.0154  | 1.02     | 0.45%              |

Settling Time Maximum Error: (Figure 2/3 in appendix)
Ideal: 1.1us		Actual: 1.03us
Maximum Settling Time Error = 6.36%
# Single Stage Results Discussion:
After changing the CMOS switch widths through parametric analysis, I achieved the single-stage switch capacitor circuit. This was consistent with different input amplitudes and had the correct amplification. As seen in the table above, I achieve all of three amplifications within 1% error. In figure 21, we can see all three stages being amplified at 4, 8, and 12. 

# Two-Stages Results
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure22.png" alt="Non-Ideal Amplifier">
</figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure23_24.png" alt="Non-Ideal Amplifier">
</figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure25.png" alt="Non-Ideal Amplifier">
</figure>

| Gain Amplification | Required Width | Actual | Expected | Maximum Gain Error |
|--------------------|----------------|--------|----------|--------------------|
| 4 × 4              | 10.8u          | 998.5  | 0.98     | 0.867%             |
| 8 × 8              | 55u            | 1.2256 | 1.22     | 0.45%              |
| 12 × 12            | 180.5u         | 1.621  | 1.62     | 0.06%              |

## Two Stage Results Discussion:
To create the results seen in the table above, I had to change the width of the NMOS in the CMOS switch in order to get the correct gain for amplification. As seen in Figure 23, even though the amplification of a gain of 4 for each amplifier, the gain for both 8 and 12 is very wrong. With the current width, the error would be well above 1%. 
Ideally, given more time, I would work to diagnose the issue. Most likely, I would need to implement each stage as an editable symbol, allowing me to control the NMOS widths in the CMOS switch after the capacitor. This would allow me to adjust each circuit to provide the best W/L ratio, eliminate any charge injection, and sample the input correctly. If this did not work, I would make the CMOS programmable, similar to the capacitor bank, and see if that yielded any results. 

# Final Results
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure26_27.png" alt="Non-Ideal Amplifier">
</figure>
<figure>
        <img src="https://raw.githubusercontent.com/AustinBostwick/534_Final_Project-/main/534Pictures/Figure28_29.png" alt="Non-Ideal Amplifier">
</figure>

| Gain Amplification | Required Width | Actual  | Expected | Maximum Gain Error |
|--------------------|----------------|---------|----------|--------------------|
| 4 × 4 × 4          | 3.1u           | 0.96429 | 0.9636   | 0.07%              |
| 8 × 8 × 8          | 11.5u          | 1.15265 | 1.15     | 0.23%              |
| 12 × 12 × 12       | 36.5u          | 1.4749  | 1.47     | 0.33%              |


# Final Result Discussion
The final results of the three-stage amplifier are similar to those of the two-stage amplifier. The only way I could achieve the gain goals was by using different widths for each gain selection. This way, I could get the maximum gain error below 1% and have the closest operational functionality. The maximum settling time was also too much. The actual settling time was slightly longer than the ideal settling time, resulting in a 7% error. 

# Conclusion
In this lab, I had many successes that allowed me to obtain a final product with some of the required specifications. As seen in my intermediate results, I was able to get the one-stage amplifier working with constant Vgs sampling, CMOS switches, and bottom plate sampling with my operational amplifier. My constant Vgs sampling had a few issues regarding random spikes, which were fixed with CMOS switches and bottom plate sampling, as discussed previously. But there are still a lot of issues that I have that prohibit me from meeting all specifications. 
	Achieving the correct gain amplification for multi-stage was my biggest problem with this lab. The amplification of the correct magnitude was only possible by changing the width of the NMOS in CMOS after the capacitor. This allowed for more efficient charge injection dissipation and circuit operationality. One of my last resorts was adding the NMOS width as a variable in the stage variables. But, simulations had the issues listed below. 
One of the biggest challenges I faced was the amount of time it took to do simulations of the two—and three-stage amplifiers. Running one simulation could take a minute or a little longer. So, running a parametric simulation to find the best values would take anywhere from 5 to 45 minutes. I tried using different servers, but it did not help. Even as I finish this report, I am trying to run simulations, but it takes more than 10 minutes to do a simple parametric. This included the software crashing multiple times. 
	I learned a lot in this lab. Combining all of the most important information from this class into one lab was very valuable for understanding of non-idealities. I think that, during our undergraduate degree, we learned very little about non-idealities. We are told to make assumptions that are not correct in the real world. But, I think this lab and class have helped me understand the importance of counting these non-idealities. I wish there were more classes like this for different subject areas. I think the most important thing I learned from this lab was the charge injection issue. Charge injection caused a lot of problems in my lab, and learning how to solve those problems was very difficult but interesting. The time-shifted correlated double sampling was also very valuable. Even though this lab was extremely difficult, I think it was very beneficial for my learning and understanding. 


