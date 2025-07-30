# retail-analytics  

📝 **Abstract**    

This Streamlit application provides an AI-powered solution for analyzing retail store videos to generate product availability summaries in 10-second time blocks. By leveraging the YOLO object detection model, the app detects and counts retail products from uploaded video footage. It segments the video into fixed time intervals and computes the average count of each product within each block. A summary table highlights products that require restocking based on their average appearance in frames. The application also visualizes detections per second and issues warnings for low-stock items. This tool is ideal for automating retail shelf monitoring and ensuring timely replenishment of inventory.  



https://github.com/user-attachments/assets/8b05170b-25c8-438e-b8de-106f5fb38fc3


**EXPLORATORY DATA ANALYSIS**

This chapter offers a detailed examination of the 'StoreItems2' dataset employed for retail product detection, its structure, preparation pipeline, and exploratory results. The characteristics of the dataset, e.g., class distribution and image features, are examined to guide model training and identify possible challenges. Visualizations are presented to support key findings, and related issues such as feature engineering are addressed.

**Dataset**
The 'retail_object' dataset, created in Roboflow, contains 1122 images of snack items from 475 different classes, e.g., 'lays american cream n onion', 'kurkure masala munch', and 'doritos nacho cheese',etc. The dataset was exported  in YOLOv8 PyTorch format and is intended for object detection applications in retail settings. 

4.1.1 **Dataset Structure and Split**
The dataset is divided into three subsets as mentioned in the 'data.yaml' file: 

**• Training Set:** 1038 images, with most of the data used for model training. 

**• Validation Set:** 43 images with 1198 instances (objects), averaging about 27.86 objects per image. 

**• Test Set:** 41 images, used for final model evaluation.

![image](https://github.com/user-attachments/assets/0988b836-07ac-4ee3-8c0f-95044cd92938)


**Preprocessing and Augmentation Pipeline**
A variety of preprocessing procedures were implemented by Roboflow to normalize the dataset:

**• Auto-orientation:** Pixel data was reformatted to adjust orientation of images, with EXIF-orientation metadata removed in order to assure consistency. 
• Resize: Each image was resized to 640x640 pixels with a stretch method to meet the YOLO model input specifications. Roboflow also applied an augmentation pipeline to generate three versions of every source 
image, effectively doubling the dataset size to 3366 images (1122 × 3). 
The augmentations used are: 
• Random crop (0–10% of the image). 

• Random rotation (±15 degrees).

• Random shear (±10 degrees both horizontally and vertically). 

• Random brightness adjustment (±15%). 

• Random exposure shift (±8%).

• Blur up to (0–2.5 pixels). 

• Noise on 2.2% of pixels.

**Algorithm Discussion** 
The YOLOv8 (You Only Look Once, version 8) algorithm is chosen due to its speed, accuracy, and appropriateness for dense retail environments. YOLOv8 is a one-stage object detection model that operates in a single pass on images, which makes it suitable for real-time applications. The algorithm breaks input images into a grid (e.g., 13x13 for a 416x416-pixel input) and predicts bounding boxes, confidence, and class probabilities for every grid cell. Objects of different sizes and aspect ratios are detected by anchor boxes to support the range of products found in retail environments.

 **Key Component**s

**● Backbone**: A convolutional neural network (CSPDarknet53) extracts multi-scale features from input images, which are important for detecting densely packed and small items. 

**● Neck:** A Path Aggregation Network (PANet) aligns features from multiple layers, improving the model's capability to detect objects at different scales. 
**● Head**: Produces predictions, such as bounding box coordinates, objectness scores (indicating object presence), and class probabilities . 

**Loss Function**

The loss function is composed of three elements: 
**● Bounding Box Regression Loss:** Utilizes Complete Intersection over Union (CIoU) loss to minimize box localization, placing bounding boxes accurately.

![image](https://github.com/user-attachments/assets/6a05dcd6-cfad-4607-9b1a-aad649e82350)


**● Objectness Loss:** 
Utilizes binary cross-entropy to improve confidence scores, separating objects from background. 
![image](https://github.com/user-attachments/assets/5c637f44-4d6c-4773-b720-8079ca0617c8)


**● Classification Loss:**
Utilizes binary cross-entropy for class prediction, reduced to a single class ("object") in this implementation.
![image](https://github.com/user-attachments/assets/e5be0481-56b0-4009-98c2-c2c4842d6f04)


**Results**

![image](https://github.com/user-attachments/assets/61dc2fcb-bc47-402b-ac75-555668679516)
![image](https://github.com/user-attachments/assets/54b5277a-7b4b-4b5c-b926-864c99a7910a)    ![image](https://github.com/user-attachments/assets/e3bf5e9f-c896-4056-81fb-a50a21719838)




**Simulation Results** 
Simulations were conducted to evaluate the model's performance in real-time retail scenarios using pre-recorded videos and images, mimicking video streams from IP cameras. The YOLOv8L model processed frames at approximately 140 FPS, effectively drawing bounding boxes around retail products with confidence scores typically ranging from 0.6 to 0.9.

Two primary simulation scenarios were tested: 

**● Shelf Monitoring**: The model successfully identified and counted products on densely packed shelves, demonstrating its utility for real-time inventory tracking. 

**● Automated Checkout:** Simulated customer interactions, where the model detected items being picked up, indicated the potential for automating checkout processes. Qualitative analysis revealed robust performance in well-lit environments. However, challenges were observed in cases of occlusion (e.g., partially hidden products) and small object detection (e.g., miniature packets), where the model sometimes missed detections or produced lower confidence scores.

![image](https://github.com/user-attachments/assets/85bacd98-63b7-4ebe-8f00-bc5bc0d61212) 
![image](https://github.com/user-attachments/assets/b2233885-fce6-44c5-9bc6-40b3ce2d6a0a) ![image](https://github.com/user-attachments/assets/4bcbcb18-b03a-460a-96ff-fbed8450f6cc)







