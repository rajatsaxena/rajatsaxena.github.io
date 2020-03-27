During non-rapid eye movement (NREM) sleep, neurons in the cortex alternate between periods of spiking (UP state) and periods of inactivity 
(DOWN state). In the current post, I have described how to detect UP and DOWN state within NREM epochs. **Acknowledgements** Dr. Justin 
Shobe for providing me the test dataset; `detect_peaks` library by Marcos Duarte. **NOTE** This script is just a starting script and 
there are other ways of going about solving the problem (`Markov models`). The script is available [HERE](https://github.com/rajatsaxena/sleep_analysis/tree/master/up-down%20states).

**Algorithm**

1. Load the raw LFP data along with the sampling rate (Fs)

2. Bandpass filter the data in 0.1-12 Hz.

3. Compute delta/theta power ratio (P_relative) to detect NREM and REM epochs. delta: 0-4Hz, theta: 7-12Hz

4. Compute epochs where the power ratio >= `mean + 0.25*std of the P_relative` and mark them as NREM epochs. Relative power threshold 
should be modified as per the data (high white line in 2nd row image, 1st row: green background=NREM, yellow=REM).

5. Load the spiking data and calculate the sum of spikes from all the cells in time bins of 25ms. Time bin-width should be tested for 
individual datasets.

6. Set an `up-state detection threshold = 5 spikes` and `falloff threshold = ` 1 spike`. 

7. Use the up-state detection threshold and falloff threshold to calculate the up-state and down-state start and end timestamps.

8. Remove putative up-states and down-state which do not lie between the duration threshold ('magenta' color in 3rd and 4th row marks 
the up-state epoch).

<img style="float: left" src="https://rajatsaxena.github.io//images/updown-states.png" width="100%" height="100%">

I hope this analysis will be a great starting point for everyone who wants to deep dive into the sleep replay literature.
