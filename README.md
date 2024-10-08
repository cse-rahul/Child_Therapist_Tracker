Overview
This project aims to detect and track people, specifically children and adults, within video frames using deep learning models. The system assigns unique IDs to each person, tracks them throughout the video, and re-identifies them if they leave and re-enter the frame. The tracking also handles post-occlusion scenarios, ensuring the correct ID is maintained even if a person is partially or completely hidden for a period.

Inference Pipeline
The inference pipeline consists of several steps, which are executed sequentially to analyze the video frames and produce the desired output:

Video Input:
The system reads video frames from an input video file using OpenCV's VideoCapture function. The video is processed frame by frame.

Preprocessing:
-Each frame is resized to a fixed width to standardize input dimensions, making the detection process consistent and efficient.
-A blob is created from the frame using cv2.dnn.blobFromImage. This blob is a preprocessed input suitable for the deep learning models.

Person Detection:
-The MobileNetSSD model, pre-trained on the COCO dataset, is used for detecting people in the frame. The model identifies objects in the frame, and we filter out non-person detections using the model's class predictions.
-The detected bounding boxes are further refined using Non-Maximum Suppression (NMS) to reduce overlapping boxes and ensure the most accurate detections are retained.

Face Detection & Age Estimation:
-Once a person is detected, the system attempts to detect the face within the bounding box using a pre-trained face detection model (res10_300x300_ssd_iter_140000_fp16).
-If a face is detected, an age estimation model (age_net.caffemodel) is applied to predict the age of the person. Based on the predicted age, the system classifies the person as a "Child" or an "Adult".

Tracking:
-The detected persons (children or adults) are tracked across frames using a centroid-based tracking algorithm. The tracker assigns unique IDs to each detected person and keeps track of their movement across the video.
-If a person exits and re-enters the frame, the system attempts to reassign the correct ID based on the centroid's proximity to previously tracked objects.

Post-Occlusion Handling:
-If a person becomes occluded (partially or fully hidden), the tracker maintains their ID. Once the person reappears, the system re-identifies them based on the previous ID assignment, ensuring continuity in tracking.

Output Generation:
-The system annotates the video frames with bounding boxes, IDs, and labels (Child/Adult) for each detected person.
The annotated frames are saved as an output video file using OpenCV's VideoWriter function, ensuring the results are easily viewable.

make sure to install module :
python -m pip install opencv-python imutils numpy scipy

Run the person_trackingg.py file for the final output result.
also you can change the test video file name in the 55 line of person_trackingg.py file cap = cv2.VideoCapture('testvideos/testvideo2.mp4') for other videos output that is already in testvideso directory
