# Object Detection: License Plates

This is the third project of the Opencv University course ["Deep Learning with PyTorch"](https://opencv.org/university/deep-learning-with-pytorch/).
It focuses on using object detection to identify license plates within a provided dataset.

## Introduction

Object detection is a task in computer vision, where the objective is to identify and localize objects within an image 
or video frame. This project focuses on detecting vehicle registration plates.

## Data

The project uses a dataset of vehicle license plate images, organized into training and validation sets with annotated 
bounding boxes.

The dataset consists of:
- 5308 images in the train set
- 386 images in the validation set 

## The method used

YOLOv5 was used to fine-tune a yolov5s model. The model was kept small with the expectation that it would perform 
adequately, allowing for the use of smaller hardware in future implementations in an edge device.
Once the data has been obtained, training a model is an extremely simple proces thanks to the [YOLOv5 training script](https://github.com/ultralytics/yolov5/blob/master/train.py).
It offers numerous features that simplify the model training process. 

This is non-exhaustive list of some of the features I noticed while exploring the script:

- **Smart optimizer:** Configures an SGD optimizer with default parameters (learning rate, momentum and weight decay) and
three parameter groups for different decay configurations for model weights, biases, and BatchNorm2d layer weights.
- **Learning Rate Scheduling:** Applies cosine learning rate decay to adjust the learning rate across epochs.
- **Data Augmentation:** Automatically applies data augmentation techniques by default to enhance model generalization.
- **Exponential Moving Average:** Tracks a smoothed version of the model weights to help with generalization.
- **Early Stopping:** Monitors validation performance and automatically stops the training if no improvement is observed.
- **Gradient Accumulation:** Supports gradient accumulation, which can help when training with batch sizes that exceed 
the available GPU memory.
- **Fine-tuning:** Allows freezing selected layers if desired, however, none are frozen by default, because the author 
[hasn't observed performance improvements from freezing layers](https://github.com/ultralytics/yolov5/issues/1264#issuecomment-721060334).
- **Automatic Mixed Precision:** Uses AMP for faster training with lower memory usage.

The hyperparameters mentioned above have sensible default values.

Understanding how YOLOv5 applies these techniques is valuable for training custom models other than YOLO.  Due to its 
AGPL-3.0 License, YOLO cannot be used in closed-source products.

## Conclusions

This solution shows a starting point that could be used in the task of automatic recognition of license plate numbers.
Without spending much time tuning hyperparameters, a base model was trained in Google Colab for only 5 epochs using
5308 images in the train set and 386 were used to evaluate the out-of-sample model performance. 

At this stage, the model can be deployed to see how it performs in real-world scenarios. If it doesn't perform as 
expected, various approaches can be explored to improve it, such as addressing overfitting or underfitting issues.

See the [notebook](Project3_object_detection.ipynb).