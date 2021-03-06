* [Install OpenCV](#install_opencv)
* [Feature Detection and Description](#Feature_Detection_Description)
* [Open Source Implementation of SIFT](#opensift)
* [How to install dlib](#dlib)
* [Structure of point locations in OpenCV functions](#structure_points_cv)
* [Extraction of x,y coordinates from OpenCV “cv2.keypoint” object](#extraction_points_from_keypoints)
* [Extraction of Patches from an Image](#patches)

# <a name="install_opencv"/>Install OpenCV
- [Install OpenCV-Python in Ubuntu](https://docs.opencv.org/master/d2/de6/tutorial_py_setup_in_ubuntu.html)
- Instal OpenCV in Ananconda:
```bash
  conda install -c conda-forge opencv
```

# <a name="Feature_Detection_Description"/>[Feature Detection and Description](https://docs.opencv.org/4.5.0/db/d27/tutorial_py_table_of_contents_feature2d.html)

Tutorials for feature detections and their descriptions

# <a name="opensift"/>[Open Source Implementation of SIFT](http://robwhess.github.io/opensift/)

# <a name="dlib"/>[How to install dlib v19.9 or newer (w/ python bindings) from github on macOS and Ubuntu](https://gist.github.com/ageitgey/629d75c1baac34dfa5ca2a1928a7aeaf)

Pre-reqs:
- Have Python 3 installed. On macOS, this could be installed from homebrew or even via standard 
  Python 3.6 downloaded installer from https://www.python.org/download. On Linux, just use your
  package manager.
- On macOS:
  - Install XCode from the Mac App Store (or install the XCode command line utils).
  - Have [homebrew](https://brew.sh/) installed
- On Linux:
  - For a full list of apt packages required, check out the [example Dockerfile](https://github.com/ageitgey/face_recognition/blob/master/Dockerfile#L6-L34) and copy what's installed there.
  - These instructions assume you are using Ubuntu 16.04 or newer. If you are using 14.04, you can try [these installation instructions instead](https://github.com/ageitgey/face_recognition/issues/120) to work around the old CMake version.
- These instructions assume you don't have an nVidia GPU and don't have Cuda and cuDNN installed and don't want
  GPU acceleration (since none of the current Mac models support this).

Clone the code from github:

```bash
git clone https://github.com/davisking/dlib.git
```

Build the main dlib library (optional if you just want to use Python):

```bash
cd dlib
mkdir build; cd build; cmake ..; cmake --build .
```

Build and install the Python extensions:

```bash
cd ..
python3 setup.py install
```

At this point, you should be able to run `python3` and type `import dlib` successfully.

## Install pre-compiled dlib package: 
If you do not want to install from source code, you can install its pre-compiled package from the <b>conda-forge</b> or <b>menpo</b> channel.
```bash
conda install -c conda-forge dlib
```
or
```bash
conda install -c menpo dlib
```

# <a name='structure_points_cv'/> [Structure of point locations in OpenCV functions](https://stackoverflow.com/questions/47402445/need-help-in-understanding-error-for-cv2-undistortpoints/47403282#47403282)

The input points need to be an array with the shape (n_points, 1, n_dimensions). So if you have 2D coordinates, they should be in the shape (n_points, 1, 2). Or for 3D coordinates they should be in the shape (n_points, 1, 3). This is true for most OpenCV functions. AFAIK, this format will work for all OpenCV functions, while some few OpenCV functions will also accept points in the shape (n_points, n_dimensions). I find it best to just keep everything consistent and in the format (n_points, 1, n_dimensions).



If you have an array that has the shape (n_points, n_dimensions) you can expand it with 

1. First Solution with **np.newaxis**:

<pre>
>>> points = np.array([[1, 2], [3, 4], [5, 6], [7, 8]])
>>> points.shape
(4, 2)
>>> points = points[:, np.newaxis, :]
>>> points.shape
(4, 1, 2)
</pre>

2. Second Solution with **np.expand_dims()**:

<pre>
>>> points = np.array([[1, 2], [3, 4], [5, 6], [7, 8]])
>>> points.shape
(4, 2)
>>> points = np.expand_dims(points, 1)
>>> points.shape
(4, 1, 2)
</pre>

3. Third Solution with **np.transpose())**:

If your shape is <pre>(1, n_points, n_dimensions)</pre> then you want to swap axis 0 with axis 1 to get <pre>(n_points, 1, n_dimensions)</pre>, 
you must use  <pre>points = np.transpose(points, (1, 0, 2))</pre> 
to put the axes 1 first, then axis 0, then axis 2.

# <a name="extraction_points_from_keypoints"/> Extraction of x,y coordinates from OpenCV “cv2.keypoint” object:

You can use:
<pre>
import numpy as np

points = np.float([kp[idx].pt for idx in range(0, len(kp))]).reshape(-1, 1, 2)
</pre>
points will be an array of keypoints.

another way:
<pre>
points = cv2.KeyPoint.convert(kp)
</pre>

# <a name="patches"/>Extraction of Patches from an Image:

- [Image patch extraction using scikit-learn](https://scikit-learn.org/stable/modules/feature_extraction.html#image-feature-extraction)
- [Pactch extraction using the Extract_patches package](https://github.com/ducha-aiki/extract_patches):
```bash
pip install extract_patches
```
