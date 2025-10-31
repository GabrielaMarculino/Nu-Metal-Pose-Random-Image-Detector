# Nu Metal Pose: Random Image Detector

> A real-time computer vision project that detects a specific hand gesture (the "Nu Metal Pose") and displays a random image on screen.

![License](https://img.shields.io/badge/License-MIT-blue.svg)

This is an open-source Python project that uses your webcam to perform real-time gesture detection. When the application identifies two open hands on screen simultaneously, it triggers an event and overlays a random image from a local folder onto the video feed.

*(Feel free to add a GIF or screenshot of your project working here!)*

## ✨ Features

* **Real-Time Hand Tracking:** Utilizes MediaPipe to track 21 keypoints on multiple hands.
* **Gesture Recognition:** Implements custom logic to determine if a hand is "open" or "closed".
* **"Nu Metal Pose" Trigger:** Specifically detects when **two** hands are simultaneously in the "open" state.
* **Random Image Overlay:** When the pose is triggered, the app randomly selects one of your custom images and displays it.
* **Transparency Support:** Correctly overlays PNG images with alpha channels (transparency).

---

## 🛠️ How It Was Made (Technology)

This project was built entirely in Python using two core computer vision libraries:

* **[OpenCV](https://opencv.org/):** Used for capturing the webcam video feed, basic image processing (like flipping the video for a "selfie" mode), color space conversion (BGR to RGB), and overlaying the final images and text onto the screen.
* **[MediaPipe](https://developers.google.com/mediapipe) (by Google):** This is the "magic" behind the gesture recognition. We use the `MediaPipe Hands` solution, which provides a high-fidelity, 21-point 3D landmark model for each hand it detects in the frame.

---

## 💡 The Logic

1.  **Capture Frame:** The script reads a new frame from the webcam using `cv2.VideoCapture()`.
2.  **Process Frame:** The frame is sent to the `MediaPipe Hands` model for processing.
3.  **Find Hands:** The model returns the landmark (keypoint) data for up to two hands.
4.  **Check Gesture:** For each detected hand, a custom function `is_hand_open()` runs. This function checks the **Y-coordinates** of the fingertips (like the index finger tip, landmark #8) against their middle joints (index finger joint, landmark #6).
    * If `tip.y < joint.y` (meaning the tip is *higher* on the screen), the finger is considered "open".
    * If 4 or 5 fingers are "open", the hand is classified as open.
5.  **Trigger Event:** If the code counts `open_hands_count == 2`, it sets a "pose detected" flag.
6.  **Pick Random Image:** A state-locking mechanism (`pose_detected_previously`) ensures that a new random image is chosen from your folder *only on the first frame* the pose is detected (this prevents the image from flickering randomly).
7.  **Overlay Image:** The chosen image is "pasted" onto the main video frame using `cv2` (OpenCV), correctly handling transparency.
8.  **Display:** The final, modified frame is shown to the user.

---

## 🚀 How to Run

### 1. Prerequisites

You must have Python 3.x installed on your machine.

### 2. Installation

1.  **Clone this repository:**
    ```bash
    git clone [https://github.com/](https://github.com/)/GabrielaMarculino/Nu-Metal-Pose-Random-Image-Detector.git
    cd [Nome da Pasta]
    ```

2.  **Install the required libraries:**
    ```bash
    pip install opencv-python mediapipe
    ```

### 3. Usage

1.  **Add Your Images:**
    * Find 5 images (or more!) that you want to display.
    * Place them in the same root folder as the Python script.
    * Update the `IMAGE_FILENAMES` list in the Python script (`main.py`) with the exact names of your files:
    
    ```python
    IMAGE_FILENAMES = [
        'my_image_1.png',  
        'my_image_2.jpg',  
        # etc...
    ]
    ```

2.  **Run the script:**
    ```bash
    python main.py
    ```
    *(Change `main.py` to whatever you named your file.)*

3.  **Make the Pose!**
    * Show both of your hands, open, to the camera to trigger the random image.

---

## 📄 License

This project is open source and distributed under the **MIT License**. This means you are free to use, modify, and distribute this code for any purpose. See the `LICENSE` file for more details.
