A lot of time cells detected during imaging have axons going outwards but we are really only interested in the soma which is circular in shape. 
So, I tried altering the shape of the cell ROIs to remove the tail but keep the center circular part. Most of the algorithm used 
below involved using different morphological transformation functions from `scikit-image`. This is the algorithm used:

- Load the ROI mask
- Close the small openings in the mask using `closing` function
- Perform `opening` operation which is basically `erosion` followed by `dilation`

This is how the output looks like. Red and black color contour represent clipped ROI and original ROI respectively.
<img src="https://rajatsaxena.github.io//images//combined_roi.png" width="50%" height="50%">

The script is available [HERE](https://github.com/rajatsaxena/suite2p_utils/blob/master/roi_shape_correction.py)
