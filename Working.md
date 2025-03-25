# MediaPipe
- Mediapipe is a open-source framework built for tasks like hand tracking,pose-estimation,face detection etc.
- It continuosly processes incoming data and extract meaningful information in real-time with minimal delay.

## mp.solutions.hands
- It is a module that provdie access to all functionalities which helps in '*hand tracking*' and '*gesture* recognition'.
### Key Components 
1. mp.solutions.hands.Hands
2. mp.solutions.hands.HAND_CONNECTIONS

### mp.solutions.hands.`Hands`
- It is class(blueprint for hand tracking) 
- mp_hands=`mp.solutions.hands.Hands(...)` is an instance.
- Parameters in the `mp.solutions.hands.Hands(...)`
  1. `'static_image_mode'`-
      - `True` for procesing ***Image***
      - `False` for ***Video***.
  2. `'min_detection_confidence'`-
      - It is the minimum confidence needed for detecting a hand.
      - Range 0 to 1.
  3. `'min_tracking_confidence'`(only when static_image_mode=False)-
       - It is the minimum confidence required to keep tracking a hand once detected.
  4. `'max_num_hands'`-
       - Maximum number of hands to track.
 - Once mp_hands object is created we can use `'.process(image)'` for detecting and tracking hand in an image or video frame using MediaPipe Hand.
 - result = `mp_hands.process(image)` returns
    1. `result.multi_hand_landmarks`: List of detected hand in a frame.
        - results.multi_hand_landmarks[0] → This represents the first detected hand.
        - We access 21 landmarks with `result.multi_hand_landmarks.landmark` for each hand
        - Each landmark is a normalized coordinate (x, y, z) in range(0,1):
            1. x : Horizontal position.
            2. y : Vertical position.
            3. z : Distance from camera.
          
    3. `result.multi_handedness`: List containing whether the detected hand is left or right.
  
 ### mp.solutions.hands.HAND_CONNECTIONS
 - It is a predefined list of landmark connections that define how hand landmarks are linked.
 - Purpose: Used to visualize hand structure by drawing lines between landmarks.
 - Ex : `mp_draw.draw_landmarks(frame, hand_landmarks, mp_hands.HAND_CONNECTIONS)`

## mp.solutions.drawing_utils
- It is a module used for drawing landmarks and annotations(connections etc.) on images.
- Key Components:
  1. mp.solutions.drawing_utils.draw_landmarks - used to draw
     - Ex :  `mp_drawing.draw_landmarks`(<Br>
                      img_rgb,  # Image to draw on <Br>
                      hand_landmarks, <Br>
                      mp_hand.HAND_CONNECTIONS,# from result.multi_hand_landmarks <Br>
                      mp_drawing_style.get_default_hand_landmarks_style(),<Br>
                      mp_drawing_style.get_default_hand_connections_style() )

## mp.solutions.drawing_styles
- It is a module in MediaPipe that provides predefined styles for drawing landmarks and connections.
-  It is mainly used with mp.solutions.drawing_utils to enhance visualization.

# RandomForestClassifier

# Testing on Live-Video
- `'cv2.VideoCapture(0)'`- opens defaulf webcam(0) for video capturing.
-  `'with mp_hand.Hands(min_detection_confidence=0.8, min_tracking_confidence=0.8) as hands:'` - is used to detect and track hand landmarks.
-  `'H, W, _ = frame.shape'` - stores Height and width as the landmarks coordinates are normalized and they are useful while drawing landmarks on frames.
-  `'frame_rgb = cv2.flip(frame_rgb, 1)'`- as webcam gives mirrored frames.so, we flip the frame horizontally(Left-Right Mirror)
- `'frame_rgb.flags.writeable = False'`- to ensure image remains unchanged while MediaPipe is processing it for hand detection.
- In `mp_drawing.draw_landmarks`
  - First `DrawingSpec` is for landmarks(the dots).
  - second `DrawingSpec` is for connections(the lines).
- `data_aux`: A flat list [x1, y1, x2, y2, ...] (used for prediction)
- `x_`, `y_`: Separate lists for calculating bounding box.
-  `if cv2.waitKey(10) & 0xFF ==  27`→  Esc button to close webcam 
