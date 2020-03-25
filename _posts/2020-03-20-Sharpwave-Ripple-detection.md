**Sharpwave ripples (SWRs)** are oscillatory patterns seen in the hippocampus (HPC) local field potential (LFP) during 
**offline periods** such as awake immobility and sleep and are associated with highly synchronous neural firing in 
the HPC. Multiple studies have now suggested that SWRs support the process of memory consolidation and retrieval. I have 
recently started to record ephys data from the HPC and the neocortex of mice during sleep. Here is an initial algorithm to 
detect SWRs using just LFP along with the python script. Note that some of the variables can change depending on the 
dataset in use. **Acknowledgements** Scott Kilianski and Aditi Bishnoi for providing me test datasets; Mekhala Kumar for 
explaining me the methodology and sharing her test scripts; `detect_peaks` library by Marcos Duarte, `neurodsp` library by 
Voytek-lab, UCSD. **NOTE** that this script is by no means perfect but it is a good start; I will keep making changes to it 
over time. The script is available [HERE](https://github.com/rajatsaxena/sleep_analysis/tree/master/ripple_analysis).

**Algorithm**

1. Load the raw LFP data along with the sampling rate

2. Bandpass filter the data in `150-250 Hz`. There is some conflict in the field on what range to use. 

3. Calculate the `RMS` power by convolving the signal with the kernel of `size=9`. 

4. Calculate the `mean rms` and `std rms` power

5. Set a `minimum and maximum ripple duration`. I have used 20ms and 200ms as the minimum and maximum SWR time.

6. Set a ripple power threshold for finding the SWR peak. I used `mean_rms + 5*std_rms` as threshold.

7. Decide a `falloff` threshold to calculate the ripple start time, end time and duration. In this case, 
`falloff_thresh = mean_rms + 0.5*std_rms` 

8. For each ripple peak, calculate the ripple starttime, endtime, duration, and peaktime. 

9. Remove putative ripple events which do not lie within the duration range. 

**Some additional comments for future analysis**

1. The analysis results can be improved by looking at spiking data recorded from all the neurons. As mentioned earlier,
SWR events are associated with increased synchronous firing. A basic spike count sum across neurons can be used as 
threshold to detect false positives and true negatives.

2. Another way to improve SWR detection is by using LFP from multiple sites. 


<img style="float: left" src="https://rajatsaxena.github.io//images//SWR.png" width="100%" height="100%">


One can argue to use different values of parameters (such as filter frequency, kernel window size, ripple power threshold, 
etc.) and I will generally agree since these are the decision that depends on the kind of dataset and current consensus in 
the field. With that said, it is easy to modify the proposed algorithm as per one's need. 

Good luck with your analysis! 


**EDIT 1**

1. I have now added SWR detection using z-scored filtered SWR envelope same as the one used by Dr Loren Frank's lab.
