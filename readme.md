# Requirement
* Python 3.5
* Numpy
* OpenCV 3.0

#### Kitti data or your own data
Tested with cmake on my Mac for C++ version, you need g2o for optimization

# Introduction
Python Files
The python file folder contains the source code with Python scripts for 2D-2D mono Visual Odometry, 3D reconstruction, and 3D-2D visual odometry. You will need to adjust the address of the source files for images accordingly.

The basic idea for 3D-2D visual odometry is to initialize the 3D points first as landmarks with template feature matching from image pairs. Then, tracking points are found using the OpenCV KLT tracking algorithm. Subsequently, landmarks, tracked points, and P3P are used to find the pose of the camera. New landmarks are continuously added over time.

Dense 3D reconstruction is achieved with the OpenCV function cv2.StereoSGBM_create to create the stereo image pair. Disparity is computed using this image pair, and depth is calculated with the provided offset by the Kitti dataset. This enables the recovery of the 3D coordinates of all points in the disparity image.

For the 2D-2D visual mono odometry case, the scale between frames is crucial since only images from one camera are available. The scale is obtained from the absolute pose data provided by the Kitti dataset. The recovered pose is then plotted.

### C++ Files
### Version 1
In version 1 of the C++ implementation, the 3D-2D visual odometry pipeline is implemented without using any optimization technique in the back-end. No additional optimization package needs to be installed for this version. The feature matching step utilizes the ORB feature extractor method. The absence of scale estimation is compensated by basing all 3D coordinates of objects on the first frame and then recovering all poses based on it.

### Version 2
In version 2 of the C++ implementation, bundle adjustment is added for optimization in the backend. The graph optimization package g2o is used for bundle adjustment. To install g2o, refer to this link for additional information.

The optimization minimizes the reprojection error between 3D points, 2D feature points, and recovered poses. In this implementation, world 3D points-landmarks are stored, and each frame observes only a subset of these 3D points. Landmark points continue to be added as new frames arrive.

You can choose to run the optimization online or offline. In the offline approach, optimization is performed after processing all frames. However, it may be slow due to the accumulation of 3D landmark points. The online approach allows optimization of only a few frames at a time while continuing processing for the remaining frames.

### How to Run

* git clone <repository_url>
For the Python files, simply run:
* python filename

Ensure you set the correct data path. Data can be downloaded here.

For the C++ files, follow these steps:

* mkdir build
* cd build
* cmake ..
* make
Then, run the executable files according to the provided instructions.

* For c_plus_version_1: vo 2000 (for 2000 frames)
* For c_plus_version_2: vo 2000 2000 1 (for 2000 frames with optimization after 2000 frames)


## Further Work
Improvements can be made by enhancing the feature extraction scheme and implementing mechanisms to handle new landmarks and remove old landmarks.
Additionally, incorporating loop detection techniques would help reduce error accumulation over time.

## References
Fall 2017 - Vision Algorithms for Mobile Robotics
