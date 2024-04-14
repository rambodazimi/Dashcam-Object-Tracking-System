# DashCam-Insights

## Introduction
In this project, we leverage computer vision to analyze traffic footage from a Montreal dashcam. We focus on two videos: one captured on McGill Drive and the other on Sainte-Catherine Street. Our goal is to detect and count cars and pedestrians in each video, further differentiating between parked and moving vehicles.

We employed the YOLOv8 object detection model for this task. YOLOv8 not only identifies objects but also estimates their speed, allowing us to distinguish parked cars from those in motion. Finally, we present the counts for both videos, including the number of parked cars passed, moving cars passed, and pedestrians detected. We'll then delve deeper into the implementation details.


## Tools and Frameworks Used

Python 3.12

OpenCV

Ultralytics (YOLOv8) – Modified version

NumPy


## Implementation Steps
To tackle this project effectively, we divided it into manageable steps: 

### Object Detection and Tracking: We began by utilizing YOLOv8 to detect and track the two primary objects of interest in each video: cars and pedestrians.

### Object Counting: Once the objects were identified, we employed YOLOv8's counting capabilities to determine the total number of cars and pedestrians present in each video.

### Parked vs. Moving Car Classification: To differentiate parked cars from moving vehicles, we leveraged YOLOv8's built-in speed estimation functionality. However, to better suit our specific needs, we customized certain functions within the model.

### Result Generation and Visualization: Finally, we processed all the data to generate the final results, including counts for each category. Additionally, we exported the output videos with visual representations of the detected objects, allowing us to present the findings to the user.


## Detailed Implementation
First, we implement a car and pedestrian tracking system using the YOLOv8 (You Only Look Once) model, a popular deep learning-based object detection algorithm. This system utilizes computer vision techniques to detect and track cars and pedestrians in a video stream. The primary objective is to annotate the detected objects in each frame and record their movement trajectories for further analysis.

We begin by initializing the YOLOv8 model, loading it from the yolov8n.pt file. Subsequently, we load the given two input videos using OpenCV's VideoCapture class. Parameters such as frame width, height, and frame rate are retrieved from the video to configure the output video file.

Furthermore, a variable named TOTAL_CARS_PEDESTRIANS_MCGILL is initialized to count the total number of cars and pedestrians detected in the video. The script then enters a frame processing loop, where it iterates through each frame of the video. For each frame, it performs object detection and tracking using the YOLO model's track method, specifically tracking cars and pedestrians (classes 0 and 2).

Within the loop, the script extracts bounding boxes and track IDs from the detection results and updates the total count of cars and pedestrians. It annotates the frame with bounding boxes and track IDs, records the movement trajectories of tracked objects, writes the annotated frame to the output video file, and displays the annotated frame to the user.

In our subsequent step, we followed a similar procedure, but with a specific focus on detecting and tracking cars only. This decision was made to enable the separate counting of both cars and pedestrians. Despite this specialization, the output video still presents detections of both categories simultaneously. By executing the process anew, we now save data regarding the total count of cars and pedestrians within each video segment.


In the next step, we initiate the processing of a video file to estimate vehicle speeds using a speed estimation module. First, we initialize the video capture object. Upon setting up the video processing pipeline, we define the position of the line used to calculate vehicle speeds within the video frame. Additionally, an instance of the speed estimation object is created, allowing for the estimation of vehicle speeds. Parameters including registration points for speed calculation, class names, and the option to visualize annotated images are set for this object.

The subsequent loop iterates through each frame of the video, processing it to estimate vehicle speeds. Initially, the loop reads a frame from the video, then utilizes the YOLOv8 model to track objects, specifically focusing on cars. The speed estimation object is then employed to estimate the speeds of vehicles in the frame, and any tracked IDs of parked cars are updated accordingly. Finally, the annotated frame, including speed annotations, is written to the output video file.

To enhance the distinction between parked and moving cars, we modified the implementation of the speed estimator within the YOLOv8 framework. This adaptation involved the introduction of thresholds to categorize each detected car based on its calculated speed. By setting specific threshold values, we could effectively differentiate between parked and moving cars. For instance, when the calculated speed is insignificantly small, it suggests that the detected car is likely moving at a similar speed to the car equipped with the dashcam. Conversely, if the calculated speed surpasses a predefined threshold, it indicates that the car is more likely parked.

However, a significant challenge arose when dealing with the McGill drive video, where cars were observed traveling in opposite directions. To address this, we needed to establish two distinct threshold values. A high calculated speed would signify that the car is moving in the opposite direction, with the speeds of the two vehicles summing together. Conversely, an average speed value would indicate that the car is more likely parked. Finally, a low calculated speed would suggest that the car is moving in the same direction as the car with the dashcam. This approach allowed us to classify cars as parked or moving. 

In the final stage, we present the outputs and results to the user in a comprehensible manner. For each input video, we produce two distinct videos: 

The first video showcases the detection and counting of both cars and pedestrians. This video provides a visual representation of the detected objects, facilitating an understanding of the traffic flow and pedestrian activity within the scene.
The second video is dedicated to calculating the speed of vehicles and determining their motion status, whether they are moving or parked. This video enables a deeper analysis of vehicle dynamics, aiding in identifying patterns of movement and distinguishing between parked and actively moving vehicles.

Following the visual presentations, we also provide users with the numerical values calculated by our model, including:

### Total Number of Cars Detected
### Total Number of Pedestrians Detected
### Parked Cars Detected
### Moving Cars Detected


## Program Performance
### Tracking and counting both Cars and Pedestrians (with generating the output video)

McGill: 1:58 minutes

St. Catherines: 2:43 minutes

### Tracking and counting Cars only (without generating any output video)

McGill: 1:37 minutes

St. Catherines: 2:11 minutes

### Speed Estimation

McGill: 2:01 minutes

St. Catherines: 2:39 minutes

### Total

McGill: 5:36 minutes

St. Catherines: 7:33 minutes
