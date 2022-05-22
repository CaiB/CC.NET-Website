---
title: "ColorChord's Music-Optimized DFT Algorithm"
description: "A specially optimized DFT with complexity of only O(n)"
lead: ""
date: 2021-01-20T00:18:44-08:00
lastmod: 2021-01-20T00:18:44-08:00
draft: false
weight: 50
contributors: ["Cai B."]
---

Algorithm devised by Will Murnane, and coded by Charles Lohr. Document written by Cai B.  
Thank you to Will for patiently helping me understand the inner workings, and thank you to Charles for dealing with my never-ending stream of questions about the software and ideas behind it.

This applies to both cnlohr's ColorChord and ColorChord.NET.

{{< alert icon="❗" text="Chromium-based browsers (e.g. Chrome, Edge) do not support MathML, so the equations will not show up correctly. Use Firefox or Safari instead." />}}

## Bold Claims
A traditional fast Fourier transform (FFT) runs in O(n*log(n)) time, where n is the number of input samples. Many implementations produce complex valued outputs.

This specially optimized discrete Fourier transform (DFT) runs in **only O(n) time**, where n is the number of input samples. Real values are output, and only the bins of interest are computed. The number of octaves and bins per octave are independently configurable, regardless of input, and are the only factor in determining how many calculations are required per input sample. The implementation is also iterative, rather than recursive like many FFTs.

## Overview
Our goal is to take an audio signal as input, and in near real-time, decompose this into frequency information. Since this is being used to visualize music, we align the output data boundaries to the chromatic scale, and analyze a fixed integer number of octaves at a fixed start frequency. The number of output bins must be constant for each octave.

In ColorChord, after the DFT step, more processing is done. All octaves in the DFT bins are combined. Bins are then turned into notes, with the chromatic scale being associated to the colour spectrum by hue. Inter-frame smoothing is applied on the outputs to keep the visualization coherent, and finally the data is output to some medium.

## Background
First, it is important to know that musical notes form an exponential series. The frequency of a note is doubled with each octave step up and halved with each octave step down (e.g. A4 is 440Hz, A5 is 880Hz, A3 is 220Hz). Octaves are divided into 12 half-steps, which comprise the chromatic scale. To calculate the frequency of the note one half-step up from a given one, you can use this formula:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>f</mi></mrow><mrow><mrow><mi>n</mi><mo>+</mo><mn>1</mn></mrow></mrow></msub><mo>=</mo><msub><mrow><mi>f</mi></mrow><mrow><mrow><mi>n</mi></mrow></mrow></msub><mo>*</mo><msup><mrow><mn>2</mn></mrow><mrow><mrow><mn>1</mn><mo>/</mo><mn>12</mn></mrow></mrow></msup></mrow></math>

&nbsp;  
Because bin frequencies may not directly correlate one-to-one with half-step note frequencies, to get the next bin’s frequency, simply replace the 12 in the exponent denominator above with the number of bins per octave.

Like a histogram, the DFT outputs data in frequency bins. Each bin contains information of a range of frequencies, rather than just the exact frequency that the bin is labelled with. In most applications, the number of bins per octave is a multiple of 12 (the number of half-steps in an octave). Integer multiples make post-processing easier, and 1*12 bins can be used if speed is a significant concern.

The basis of operation of the Fourier transform is that all signals are a combination of sine waves. The transform uses the composite input signal and works backwards to give us information about all the input sine waves contained within. This works because the components are orthogonal (that is to say, they are basis functions). The inner product is taken to do this operation, as so:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>a</mi></mrow><mrow><mi>b</mi></mrow></msub><mo>=</mo><msubsup><mrow><mo>&#8747;</mo></mrow><mrow><mrow><mo>-</mo><mi>&#8734;</mi></mrow></mrow><mrow><mrow><mi>&#8734;</mi></mrow></mrow></msubsup><mrow><mi>x</mi><mo>(</mo><mi>t</mi><mo>)</mo><mo>*</mo><mi>b</mi><mo>(</mo><mi>t</mi><mo>)</mo></mrow><mi> </mi><mi>d</mi><mi>t</mi></mrow></math>

&nbsp;  
Where x(t) is the composite input signal, b(t) is the basis function (sine wave at a specific frequency), and a_b becomes the amount of the basis function in the input signal (i.e. the amount of that particular sine wave in the composite signal). By repeating this operation for each sine wave frequency, we are able to find the amount of each of those frequencies in the composite signal.

## Algorithm
We analyze each frequency bin in comparison to the input signal, by multiplying a single sine wave at the bin frequency by the input signal, making sure that both are the same length (sample count). Let’s start simple by assuming our input is simply another single sine wave, with frequency ωinput. This multiplication would in effect look something like this:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>)</mo><mo>*</mo><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>b</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>)</mo></mrow></math>

&nbsp;  
We now sum the output of this multiplication at every sample in our window to see how well correlated they are. The window is considered as the previous n number of samples, ending at the most recently acquired sample.

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>=</mo><msubsup><mrow><mo>&#8721;</mo></mrow><mrow><mrow><mi>t</mi><mo>=</mo><mn>0</mn></mrow></mrow><mrow><mrow><mi>t</mi><mo>=</mo><msub><mrow><mi>t</mi></mrow><mrow><mrow><mi>m</mi><mi>a</mi><mi>x</mi></mrow></mrow></msub></mrow></mrow></msubsup><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>)</mo><mo>*</mo><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>b</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>)</mo></mrow></math>

&nbsp;  
(where t_max is the last, most recent, sample in our window)

If we now look at the case where the input’s frequency matches one of our bins, we can simplify this:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>=</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>b</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>=</mo><mi>&#969;</mi></mrow></math>
&nbsp;
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><mi>sin</mi><mo>(</mo><mi>&#969;</mi><mo>*</mo><mi>t</mi><mo>)</mo><mo>*</mo><mi>sin</mi><mo>(</mo><mi>&#969;</mi><mo>*</mo><mi>t</mi><mo>)</mo><mo>=</mo><msup><mrow><mi>sin</mi></mrow><mrow><mn>2</mn></mrow></msup><mo>(</mo><mi>&#969;</mi><mo>*</mo><mi>t</mi><mo>)</mo></mrow></math>
&nbsp;
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>=</mo><msubsup><mrow><mo>&#8721;</mo></mrow><mrow><mrow><mi>t</mi><mo>=</mo><mn>0</mn></mrow></mrow><mrow><mrow><mi>t</mi><mo>=</mo><msub><mrow><mi>t</mi></mrow><mrow><mrow><mi>m</mi><mi>a</mi><mi>x</mi></mrow></mrow></msub></mrow></mrow></msubsup><msup><mrow><mi>sin</mi></mrow><mrow><mn>2</mn></mrow></msup><mo>(</mo><mi>&#969;</mi><mo>*</mo><mi>t</mi><mo>)</mo></mrow></math>

&nbsp;  
When ωinput matches ωbin, this creates the most overlap, and as such the largest sum. When the frequencies match perfectly, the sum is at a maximum. As the frequencies get further and further apart, the signals get more and more out-of-sync, and so the sum approaches 0. 

Another problem is that the input may not be in phase with our bin signal. We avoid this by using two different reference signals at the same frequency. One will be orthogonal to the other, i.e. 90 degrees out of phase, hence a cosine. This way, we can use these two sums to analyze an input signal at any phase offset.

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>c</mi><mi>o</mi><mi>s</mi></mrow></mrow></msub><mo>=</mo><msubsup><mrow><mo>&#8721;</mo></mrow><mrow><mrow><mi>t</mi><mo>=</mo><mn>0</mn></mrow></mrow><mrow><mrow><mi>t</mi><mo>=</mo><msub><mrow><mi>t</mi></mrow><mrow><mrow><mi>m</mi><mi>a</mi><mi>x</mi></mrow></mrow></msub></mrow></mrow></msubsup><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>+</mo><msub><mrow><mi>&#966;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>)</mo><mo>*</mo><mi>cos</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>b</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>)</mo></mrow></math>
&nbsp;
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>=</mo><msubsup><mrow><mo>&#8721;</mo></mrow><mrow><mrow><mi>t</mi><mo>=</mo><mn>0</mn></mrow></mrow><mrow><mrow><mi>t</mi><mo>=</mo><msub><mrow><mi>t</mi></mrow><mrow><mrow><mi>m</mi><mi>a</mi><mi>x</mi></mrow></mrow></msub></mrow></mrow></msubsup><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>+</mo><msub><mrow><mi>&#966;</mi></mrow><mrow><mrow><mi>i</mi><mi>n</mi><mi>p</mi><mi>u</mi><mi>t</mi></mrow></mrow></msub><mo>)</mo><mo>*</mo><mi>sin</mi><mo>(</mo><msub><mrow><mi>&#969;</mi></mrow><mrow><mrow><mi>b</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mo>*</mo><mi>t</mi><mo>)</mo></mrow></math>

&nbsp;  
If an input signal were 180 degrees out of phase, then m_sin would be at its largest, but negative. If it were 45 degrees, we would see significant output on both m_sin and m_cos. Hence, we get the magnitude of these two components in order to get a correct representation of the phase-independent correlation:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><mi>m</mi><mo>=</mo><msqrt><mrow><mrow><mo>(</mo><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><msup><mrow><mo>)</mo></mrow><mrow><mn>2</mn></mrow></msup><mo>+</mo><mo>(</mo><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>c</mi><mi>o</mi><mi>s</mi></mrow></mrow></msub><msup><mrow><mo>)</mo></mrow><mrow><mn>2</mn></mrow></msup></mrow></mrow></msqrt></mrow></math>
&nbsp;
{{< alert icon="💡" text="Potential Optimization: Because we often don’t care about the precise value of m, an approximation of the square root often provides adequate output precision and can be significantly faster on some platforms. Alternatively, an approximation for the norm may also be used, potentially for further performance gain." />}}

&nbsp;  
If phase information is also desired, it can be extracted as such (although this is usually not useful):
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><mi>&#966;</mi><mo>=</mo><mi>a</mi><mi>t</mi><mi>a</mi><mi>n</mi><mn>2</mn><mo>(</mo><mi>x</mi><mo>=</mo><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi></mrow></mrow></msub><mi>,</mi><mi>y</mi><mo>=</mo><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>c</mi><mi>o</mi><mi>s</mi></mrow></mrow></msub><mo>)</mo></mrow></math>

&nbsp;  
Now we would repeat this step at every bin in one octave. The default ColorChord implementation uses 24 bins per octave, 2 for every note. This gives us information about how much of each of those bins is present in the input signal.

This operation can be run at every single input sample to get a fast-updating output, or only every n samples if utmost update speed is not required, or processing power is limited. Overlap in the windows of successive runs is acceptable and provides some natural smoothing to the output values.

Choosing window size is a careful balance. Too short, and output peaks become very wide with significant sinc-shaped ringing around the center peak, introducing noise in later processing. (This is a result of the transform not having enough information to rule out these other possibilities.) Too long, and you have significant delays and persistence, and more processor power and memory may be required. 

&nbsp;  
{{< alert icon="💡" text="Potential Optimization: Instead of storing (or even worse, calculating) m_sin and m_cos values for each bin for the entire window every time a sample is added, a single average history value along with the most recent sample can be used for each bin, and an IIR (infinite impulse response) filter can be used to significantly reduce memory requirements. Use a number close to, but under 1 for the IIR filter value c_IIR, e.g. 0.9." />}}
  
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mrow><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi><mi>a</mi><mi>v</mi><mi>g</mi></mrow></mrow></msub><mo>=</mo><mo>(</mo><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi><mi>a</mi><mi>v</mi><mi>g</mi></mrow></mrow></msub><mo>\*</mo><msub><mrow><mi>c</mi></mrow><mrow><mrow><mi>I</mi><mi>I</mi><mi>R</mi></mrow></mrow></msub><mo>)</mo><mo>+</mo><mo>(</mo><msub><mrow><mi>m</mi></mrow><mrow><mrow><mi>s</mi><mi>i</mi><mi>n</mi><mi>n</mi><mi>e</mi><mi>w</mi></mrow></mrow></msub><mo>\*</mo><mo>(</mo><mn>1</mn><mo>-</mo><msub><mrow><mi>c</mi></mrow><mrow><mrow><mi>I</mi><mi>I</mi><mi>R</mi></mrow></mrow></msub><mo>)</mo><mo>)</mo></mrow></math>

&nbsp;  
&nbsp;  
Once we have calculated one octave, there is a technique we can apply to make getting the others faster, rather than just repeating this procedure for more bins. Take 2 of the input samples, and average them together, then repeat for the entire window. This will create a new set of samples, with a window size of half of the original (as 2 samples become 1), but we will treat this new window at the same sample rate as the original. This procedure can also be thought of as speeding up the signal to 200% of the original, without a change of sample rate. At this point, we simply use the exact same sin and cos tables we already have for the original octave, except this time, they will give us results for an octave below the original.

This works because a component in the input signal that was previously an octave too low, got sped up by a factor of 2, pitching the audio up exactly one octave. Note that our window size is now effectively halved, so only a part of the sin and cos tables are multiplied against. We do not use the same window size, as this would mean that lower octaves have more persistence (effectively having a 2x longer window in real-time), which could distort our output.

&nbsp;  
{{< alert icon="💡" text="Potential Optimization: Because window sizes are halved, this means that our summing operation only takes in half as many sample points. Instead of multiplying by 2 to get the same level, we can simply add the two samples when moving an octave down, which has the advantage of counteracting the reduction in sensitivity, while also reducing processing needed both during (addition instead of addition and division) and after the change (no additional operation instead of multiplication by 2)." />}}

&nbsp;  
Note that this lower octave only effectively gets a new sample every other input sample. This means that the lower octave only needs to be computed (updated) every 2 input samples.

This process is repeated for each lower octave, until the full range of desired frequencies is achieved. This means that each octave below the original is only updated at half the rate of the octave above it (e.g. root (top) octave is updated every sample, next octave down every 2 input samples, next octave below that every 4 input samples, and the one below that every 8 samples, etc.)

Note that this means that each lower octave has half the window size, and as such it is important to choose the root (top) octave’s window size to be large enough to make sure analysis in the lower octaves provides data of sufficient quality. Specifically, smaller window size causes peaks to be wider and create larger side-peaks, as is common to all Fourier transforms. 

This also means that the amount of computations done is not constant with every input sample. The worst case (every 2^n samples, where n is [number of octaves -1]) may require significantly longer than the best case (every other sample). If this inconsistency is unacceptable, or the worst case takes too long and upsets sampling on a processing power limited device, the updates can be staggered. This way, the bins may not be perfectly in sync for where their data comes from, but the processing load per sample can be made reasonably constant.

&nbsp;  
{{< alert icon="💡" text="Potential Optimization: The octaves that need to be updated on a non-staggered system with each sample can be easily retrieved from the LSB (least-significant bits) region of a counter that increments with each incoming sample. The lowest bit being on corresponds to the second-highest octave needing to be updated, the second-lowest bit corresponding to the third-highest octave, etc. This way no additional tracking is required of the octaves requiring updating at each sample." />}}

&nbsp;  
## Post-Processing
Once all the bins are computed, the DFT has finished its work. ColorChord’s requirements mean a significant amount of post-processing is required on this data before it can be shown in visualization. One of the main steps that needs to be done is to extract 2 pieces of information from every peak found in the output bins: the relative amplitude, and the peak frequency.

Peak frequency can be more accurately determined by looking in adjacent bins. By comparing the bins to the left and right, we can approximate the actual location within the center bin of the peak.

E.g. if a local maximum falls in a given center bin, and the bin to its left has little content, but the bin to the right has significant content, we can approximate that the peak is not in the middle of the center bin, but actually at a somewhat higher frequency (towards the right bin). Using ratios gives a numerical representation of this location.

In some applications, frequency spread is also an important consideration. If an input signal has frequency content at two locations that fall into the same or close bins, by only looking at center and total amplitude, it becomes impossible to distinguish between this case, and a case where only one component is present in between the two. In music, which ColorChord is optimized for, this is not common, as it would not sound pleasant. Other applications may also want to consider how wide (in terms of frequency spread) a given peak is and consider whether a given peak is one component or multiple.

Notice that as mentioned earlier, the lower octaves have smaller windows, and as such a pure input at these lower frequencies creates more spread due to less data being available for analysis at lower octaves.

---

Equations generated with help from [CodeCogs Equation Editor](https://www.codecogs.com/latex/eqneditor.php).