During non-rapid eye movement NREM) sleep, previously stored patterns are spontaneously reactivated and interleaved in the 
hippocampus (HPC) and neocortex (NC). One of the characteristics of cortical local field potential (LFP) recorded using 
electrode during NREM sleep stage II is K-complexes (KC). KC consists of a brief negative high-voltage peak typically 
occurring spontaneously that is often followed by bursts of sleep spindles. In the current post, I have described how to go 
about detecting KC events. **Acknowledgements** Dr. Justin Shobe for providing me the test dataset; Scott Kilianski and 
Dr. Shobe for discussion of the algorithms; `detect_peaks` library by Marcos Duarte. **NOTE** this script is just a version1 
and can be significantly improved. I will keep making edits to it now and then. The script is available [HERE](https://github.com/rajatsaxena/sleep_analysis/tree/master/kc_analysis).

**Algorithm**

1. Load the raw LFP data along with the sampling rate (Fs)

2. Bandpass filter the data in `0.1-7 Hz` to get slower oscillations. Different research groups use different frequency ranges.

3. Calculate the `mean amplitude` and `std amplitude` on the filtered signal.

4. Set a `minimum and maximum KC duration`. I have used 10ms and 1.25s as the minimum and maximum KC time.

5. Set a signal amplitude threshold on the filtered signal for finding the KC event peak. I used `mean_amp + 2.25*std_amp` as threshold.

6. Once the peaks are detected, decide a `falloff` threshold to calculate the KC start time, end time and duration based on KC 
event peaks. In this case, I used `falloff_thresh = mean + 4*std' of all the KC peaks amplitudes.

7. For each KC event peak, calculate the KC starttime, endtime, duration, and peaktime. 

8. Remove putative KC events which do not lie within the duration range set earlier.

9. Calculate the sum of high gamma power (>50Hz) using spectrogram analysis for the entire sleep duration.

10. Store only those KC events that have epochs with `high gamma power < mean - 0.5*std high gamma power` within their 
event boundary. Remove other KC events. 

**Some additional comments for future analysis**

1. The analysis results can be improved by looking at spiking data recorded from all the neurons. Integrating KC LFP-based 
event detection with spiking up and down states can significantly improve the entire algorithm. A basic spike count sum 
across neurons can be used as threshold to detect up and down states.

<img style="float: left" src="https://rajatsaxena.github.io//images/KC.png" width="100%" height="100%">

This analysis by no means is the best out there and solves the problem. That is why I request everyone to take a look at
their datasets and decide for themselves on how to modify the proposed algorithms. Good luck with your analysis! 
