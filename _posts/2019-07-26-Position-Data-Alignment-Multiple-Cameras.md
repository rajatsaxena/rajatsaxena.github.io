---
layout: post
title: "Position Data Alignment from Multiple Cameras"
date: 2019-07-26
---

How do we go about aligning x,y position coordinates from eight different cameras covering different parts of the same environment? We are going to use a LED to calculate the perspective transformation between all pairs of cameras and align them in single world coordinate system. Let's jump into the setup then. We have eight raspberry pi cameras covering a large room (16m<sup>2</sup>). This is how each camera's field of view overlap region looks in 2D plane (Saxena et al. 2018):

<img src="https://rajatsaxena.github.io//images//CameraFOV.png" width="75%" height="75%">

Here are the steps used in calculating the aligned camera position:

1. Run position tracking script on each camera individually. ENSURE that when you are running position tracking, you remove outside regions from each camera FOV to avoid reflection. Reflections can cause error in homography estimation. **Summing it up, define the ROI on each camera such that no reflection zones are present.**

2. Load the position data and find unoccupied frames in each camera. To begin with, select position data and unoccupied frame data from any two cameras (say camera 2, 3). 

3. Find frames where LED is visbile in both the cameras and store the intersecting X,Y position for each of them in an array. 

4. Use the intersecting X,Y position to create source camera (camera2) and destination camera coordinates (camera3). These intersecting points are going to serve the same function as the keypoints detected using feature transformation. **NOTE that the source camera coordinates will be transformed to destination camera coordinates reference frame.**

5. Apply RANSAC algorithm on intersecting X,Y coordinates to remove geometrically inconsistent matches and improve homography estimation (also nice way to remove noisy reflection data).

6. Find homography matrix using the intersecting X,Y coordinates between the two cameras.

7. Using the homograhy matrix, perform the perspective transformation to transform source point (camera 2) to destination point (camera 3) coordinate system such that they are now aligned.

8. The merged camera coordinates between the two *merged_cam23_X, merged_cam23_Y* is equal to the mean of transformed X and Y position of the two cameras. 

9. Generate the aligned position data for one half of the *merged_cam1234_posX, merged_cam1234_posY* room by repeating the above steps with the merged camera position and the next camera position (say camera 1) followed by the same steps with the next camera (camera 4).   

10. Rerun the above steps to get aligned position data for the other half of the room *merged_cam5678_posX, merged_cam5678_posY* 

12. Find the intersecting coordinates, followed by calculating the homography matrix and perspective transformation between one half and other half of the camera coordinates to merge them into single merged coordinates *merged_allcams_posX, merged_allcams_posY*.

13. Finally, save all the processed data into a matfile

***WARNING*: The homography estimation vary as the function of number of intersecting coordinates between cameras. Less number of intersecting coordinates will lead to incorrect estimation. Although once the homography matrices are estimated, you can use them for data collected across multiple days as long the cameras are not moved. **

The script is available [HERE](https://github.com/rajatsaxena/MultiCameraPositionAlignment)

<img src="https://rajatsaxena.github.io//images//cam1234.PNG" title="Camera1234" width="75%" height="75%">

<img src="https://rajatsaxena.github.io//images//cam5678.PNG" title="Camera5678" width="75%" height="75%">

<img src="https://rajatsaxena.github.io//images//allcameras.PNG" title="AllCameras" width="75%" height="75%">
