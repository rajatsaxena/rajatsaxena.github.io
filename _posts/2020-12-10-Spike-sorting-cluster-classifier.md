I am going to use this post to document my work with <a href="https://www.janelia.org/lab/pachitariu-lab" target="_blank"> Dr. Marius Pachitariu Lab </a> located at Janelia Research Campus over the summer and Fall 2020.  The project aimed to build a classifier to label cluster units output by the <a href="https://github.com/MouseLand/Kilosort" target="_blank"> kilosort2 </a> program as either `1) good/mua or 2) noise units` using cluster waveforms and the autocorrelograms (acg). Before I begin, I would like to Dr. Marius Pachitariu to provide me the opportunity and guidance, Dr. Nick Steinmetz and Dr. Josh Siegel for sharing the data, and the entire kilosort2 team. 

**Background**


Traditionally, electrophysiologists have been using tetrodes (four electrode wires twisted together) to record electrical signals from an ensemble of neurons. These tetrodes (typically 18) are loaded on a hyperdrive, which is then implanted in the animal's brain. Multiple electrodes in a tetrode record different looking waveforms for each spike fired by the neurons in the electrodes' vicinity. The waveforms on each electrode in a tetrode differ in varying features calculated for each spike, such as peak amplitude, energy, and valley amplitude because of the geometric configuration of the electrodes. The electrodes near a neuron might pick up high amplitude spikes compared to the electrodes farther away from that neuron. Spike sorting technique assigns spikes to different neurons or units and distinguishes spiking from background electrical noise using the shape of waveforms collected with one or more electrodes. Most labs have their custom build manual spike sorter that takes hours (sometimes days) of human time to process through the entire data. 


**WORK IN PROGRESS**