
## Table of Contents
- [Installation and Requirements](https://github.com/x-sl0w/Object_Detection_in_Python_using_YOLOv3#installation-and-requirements)
- [Making Custom Dataset](https://github.com/x-sl0w/Object_Detection_in_Python_using_YOLOv3#making-custom-dataset)
- [Training new weights for Oject Detection](https://github.com/x-sl0w/Object_Detection_in_Python_using_YOLOv3#training-new-weights-for-oject-detection)
- [How to run this program](https://github.com/x-sl0w/Object_Detection_in_Python_using_YOLOv3#how-to-run-this-program)
- [Results](https://github.com/x-sl0w/Object_Detection_in_Python_using_YOLOv3#results)
- [References](https://github.com/x-sl0w/Custom-Object-Detection-in-python-using-YOLOv3#references)

## Installation and Requirements

[Darknet installation](https://pjreddie.com/darknet/install/)

**Requirements:**
- CUDA 10.1
  - [Installation Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
- OpenCV
  - `$ sudo apt-get install python-opencv`

## Making Custom Dataset

**Objects:**
1. Tomato
2. Orange
3. Banana

Just took about 180 pictures (160 for training and 20 for testing) of objects above, and label them with [lableImg tool](https://github.com/tzutalin/labelImg).


## Training new weights for Oject Detection

I have decided to train weights using [yolov3-tiny](https://github.com/pjreddie/darknet/blob/master/cfg/yolov3-tiny.cfg) config , beacause of low GPU memory problem (NVIDIA GTX 960 2gb), I have only 2gb video memory, when at least 2-3gb required for training on YOLOv3-320, 416, 608

**How to train tiny-yolo to detect custom objects:**

1. Download [Default Weights](https://pjreddie.com/media/files/yolov3-tiny.weights) of yolov3-tiny
2. Get pre-trained weights yolov3-tiny.conv.15 using command: 

`./darknet partial cfg/yolov3-tiny.cfg yolov3-tiny.weights yolov3-tiny.conv.15 15`

3. Make custom model yolov3-tiny-obj.cfg based on cfg/yolov3-tiny_obj.cfg
4. Then Train: 

`./darknet detector train data/obj.data yolov3-tiny-obj.cfg yolov3-tiny.conv.15 -map`

5. Stop training when _average loss_ will be less than 1 **(0.863960 here)**


### Some updates on yolov3-tiny.cfg
1. batch = 16
2. subdivision = 2
3. width = 416, height = 416
4. steps = 4800, 5400
5. classes in each [yolo] layer is equal to 3, because i have only 3 classes
6. filter in each [yolo] layer is equal to 24, because filter = (classes + 5) * 3
7. create obj.names and obj.data files
8. create train.txt and test.txt (path for each image)

## How to run this program

- for **images:**

`python app.py --img=test/<image.jpg> --out=output/<result.jpg>`

- for **video:**

`python app.py --video=test/<video.mp4> --out=output/<result.mp4>`

## Results
<img src="/test/test_image.png" width="400"/> <img src="/output/test_result.png" width="400"/> 


## References

1. [GitHub(pjreddie) of Darknet framework](https://github.com/pjreddie/darknet)
2. [Website of the Darknet framework and YOLOv3](https://pjreddie.com/darknet/)
3. [GitHub(AlexeyAB) with a lot of information about Darknet framework](https://github.com/AlexeyAB/darknet)
4. [Adrian Rosebrock, Yolo Object Detection With Opencv, PyImageSearch](https://www.pyimagesearch.com/2018/11/12/yolo-object-detection-with-opencv/)
5. [Adrian Rosebrock, Intersection over Union (IoU) for object detection, PyImageSearch](https://www.pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/)
6. [The Ancient Secrets of Computer Vision](https://www.youtube.com/playlist?list=PLjMXczUzEYcHvw5YYSU92WrY8IwhTuq7p)
