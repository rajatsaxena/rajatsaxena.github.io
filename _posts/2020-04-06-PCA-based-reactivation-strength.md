I was recently reading a paper titled [Replay of rule-learning related neural patterns in the prefrontal cortex 
during sleep](https://www.ncbi.nlm.nih.gov/pubmed/19483687) by Peyrache et al. 2009. It is an amazing paper that 
demonstrates memory trace reactivation during sleep in the medial Prefrontal Cortex (mPFC) and how it is coupled 
to the hippocampus sharp-wave ripple (SWRs). This is a must paper to read for anyone interested in the hippocampus-cortical 
interaction and reactivation during sleep.

The paper uses a PCA-based analysis to compute reactivation strengths. This post is just an attempt to try to replicate the 
algorithms used in the paper.

**Algorithm**

There are two parts of the algorithm: **Part I: Template Epoch**

1.Get the spike raster for the cell ensembles.
<img style="float: left" src="https://rajatsaxena.github.io//images/spikeraster.png" width="100%" height="100%">

2.Bin the spike trains in 25ms bin. The authors use 100ms bins. Binned spike trains are then z-transformed. 
<br />
<img style="float: left" src="https://rajatsaxena.github.io//images/binnedspikeraster_Z.png" width="100%" height="100%">

3.Compute the correlation matrix of the z-transformed binned spike trains.
<br />
<img style="float: left" src="https://rajatsaxena.github.io//images/correlationmatrix.png" width="100%" height="100%">

4.Compute the principal components of the correlation matrix.
<br />
<img style="float: left" src="https://rajatsaxena.github.io//images/PCcorr.png" width="100%" height="100%">

5.Compute PCA projector operators P1,P2....,Pn as the external product of the component vector with itself. The sum 
of these projector operators, weighted by their respective eigenvalues, yields the correlation matrix (shown in 
right). *The projector matrices will be used as our templates.*
<br />
<img style="float: left" src="https://rajatsaxena.github.io//images/Templates.png">
<img style="float: left" src="https://rajatsaxena.github.io//images/PCCorrdecoded.png">

**Part II: Target Epoch**

1.Get the binned spike train from sleep epochs or epochs that you want to match templates to. 

2.Each template computed above is separately applied on the target epoch: for each time t the 
binned spike trains is left and right multiplied on the projector matrix. This gives us the reactivation
strength at time t between the population vector (binned spike count at time t) and the template.

3.Repeat this process  for each time bin in the target epoch. This will yield a time course for the reactivation
strength for each time bin in the target epoch, yielding a time course for the reactivation strength. 
Reactivation strength computed from two randomly chosen templates:
<br />
<img style="float: left" src="https://rajatsaxena.github.io//images/reactivationstrength.png"  width="100%" height="100%">

One can now compare the reactivation strength between NREM, REM epochs or pre-task/ post-task sleep. There are multiple other ways to look at reactivation which I will post about in future. Good luck! 
