# MF-SORT

## Introduction
This repository contains an unofficial implementation of the Simple Online and Realtime Tracking with Motion Features (MF-SORT) algorithm, as it was presented in (Fu et al, 2019). It is an extension of the original [SORT](https://github.com/abewley/sort) algorithm using Mahalanobis distance and a matching cascade similar to that of [DeepSORT](https://github.com/nwojke/deep_sort).

I am not the original author of MF-SORT, though this repository represents a close implementation based on the description given in the original publication. See [Fu et al, 2019](https://doi.org/10.1007/978-3-030-34120-6_13) for further details on the algorithm.

## Dependencies
This code was developed and tested on Python 3.6, though it should run on any recent version of Python (3+). The following dependencies are needed to run the tracker:

* Numpy
* Scipy

In addition, in order to run the demos contained in the `examples` folder, the following dependencies are required:

* TQDM (for progress bars)
* OpenCV (for visualization only)
* imutils (for visualization only)

## Installation and Demo
To install MF-SORT, clone the repository and install the package using pip.

```
git clone https://github.com/kbvatral/MF-SORT.git
cd MF-SORT
pip install .
```

The repository contains one minimum working example using a video file and an associated CSV file of object detections found in the `examples` folder. The object detections were generated through YoloV3, though any object detector could be used.

To run tracking on the demo video with the provided detections, simply run `examples/track.py`, which will produce a CSV file containing the generated tracks in the example data directory. To visualize the generated tracks, simply run `examples/visualize_track.py`, which will produce and AVI video file which shows the original video with tracking bounding boxes overlaid.

All detections and generated tracks conform to the formatting of the [MOT16/17 Challenge Benchmark](https://motchallenge.net/).

## Using MF-SORT
The API is divided into two main classes: `Detection` and `MF_SORT`. Detection is a wrapper to contain the information (bounding box and class score) for the detections generated by any object detection framework. MF_SORT is the actual multiple object tracker which takes in these detections frame by frame and produces the tracks. MF_SORT has a predict/update loop interface that should be familiar to anyone who has worked with Kalman filters in the past. 

```
from mf_sort import Detection, MF_SORT

# Create instance of MF_SORT with the default params
mot = MF_SORT()

# Generate detections from any object detection framework
detections = []
for ...:
  x, y, w, h, score = ...
  det = Detection([x,y,w,h], score)
  detections.append(det)

# Run MF-SORT on new detections
mot.predict()
trackers = mot.update(detections)

# Trackers is a list of tuples where each entry is in the format (mf_sort.Detection, track_id)
```

The file `examples/track.py` contains a complete working example of this process in an offline format (i.e. detections are coming from an existing file) and should provide sufficient information about the use of the API to get started with your own project.

## Acknowledgements and License
* All credit for the MF-SORT algorithm goes to the original authors. Please see their publication [Fu et al, 2019](https://doi.org/10.1007/978-3-030-34120-6_13) for further details
* Parts of this repository were inspired by or taken directly from the repositories for [SORT](https://github.com/abewley/sort) and [DeepSORT](https://github.com/nwojke/deep_sort), both licensed under GPL-3.0
 * This repository is also licensed under GPL-3.0 
