# **CS 423: Project Plan**

Zohayr Asym, Tolga Gümüşten, Berrin Aydoğmuş

## **SmartRPS – Real-Time Hand Gesture Game with Adaptive AI** **Abstract**

This project presents _SmartRPS_, a real-time, camera-based Rock-Paper-Scissors game controlled
entirely through hand gestures and powered by a behavior-learning AI opponent. The system
uses computer vision techniques to detect and classify hand gestures (Rock, Paper, Scissors) in
real time via a standard webcam, leveraging Google MediaPipe for hand landmark extraction.
Unlike conventional RPS implementations where the AI plays randomly, SmartRPS features an
adaptive AI that analyzes the player's historical move patterns using a frequency-based Markov
predictor and adjusts its strategy accordingly. The system additionally supports a two-player
mode in which both players' hands are detected simultaneously from a single camera feed. A
real-time statistics dashboard displays win rates, move history, AI confidence scores, and
behavioral analysis. The expected outcome is a fully functional, low-latency interactive game
that demonstrates practical applications of hand detection, gesture classification, and player
behavior modeling in computer vision.

## 1. Introduction


Hand gesture recognition is a fundamental problem in computer vision with broad applications
in human-computer interaction, augmented reality, and intelligent systems. Real-time gesture
classification from a live video stream requires robust hand detection, accurate landmark
estimation, and efficient classification — all of which are core topics covered in CS 423.

The motivation for _SmartRPS_ is to go beyond a simple gesture detector and build a complete
interactive system that is both technically challenging and publicly demonstrable. Rock-PaperScissors provides a well-defined gesture vocabulary (three distinct hand poses) that is ideal for
benchmarking classification accuracy, while the adaptive AI component introduces a player
modeling problem — predicting human behavior from historical data — which extends the
project beyond standard CV pipelines. The primary contributions of this work are:

**(1)** a real-time hand landmark detection and gesture classification pipeline supporting
simultaneous two-hand tracking;
**(2)** a frequency-based Markov predictor that models player behavior and adapts AI strategy
over time;
**(3)** a two-player mode enabling competitive play between two users in front of a single camera;
**(4)** a live statistics dashboard providing gesture confidence scores, win/loss history, and AI
behavioral analysis.


## **2. Method**

The SmartRPS system is composed of four main modules: hand detection, gesture classification,
adaptive AI, and the game engine with dashboard.

**A. Hand Detection & Landmark Extraction:** Google MediaPipe Hands is used to detect 21 3D
hand landmarks on each detected hand in real time on a typical 640 x 480 web camera feed at
30 frames per second. MediaPipe can detect up to two hands at once, thus permitting the twoplayer mode. Each landmark has normalized (x, y, z) coordinates with respect to the wrist and
thus the representation is independent of hand size and location within the frame.

**B. Gesture Classification:** Geometric rules based on the position of landmarks are used to
classify gestures. On each finger we calculate the relative position of the fingertip landmark
(above or below the knuckle landmark) to tell us whether the finger is straight or bent. The
three classes of gestures can be characterized as follows:

- Rock: Five fingers curled all-- all fingertips below their knuckles.

- Paper: Five fingers spread out-- fingertips over the knuckles.

- Scissors: Index/middle fingers straight; ring, pinky, and thumb curled.

Each prediction is calculated using a confidence score, which is the fraction of landmarks that fit
the desired configuration, which is displayed as a measure of reliability in the UI.

**C. Adaptive AI - Frequency-Based Markov Predictor:** The AI opponent keeps a history of the
previous N moves of the player (default N=20) and calculates the empirical probability
distribution of the three gestures. It will then pick the move that defeats the gesture most
played by the player. As an example, when the player has played Rock in 60% of last few
rounds, the AI will be biased towards Paper. This forms a dynamic challenge curve - the AI
grows increasingly challenging with more behavioral data. During the initial rounds (less than 5
moves recorded), the AI resorts to the use of uniform random selection to allow fair play at the
beginning.

**D. Two-Player Mode:** In two players mode, MediaPipe tracks both hands at the same time in
one camera frame. Player 1 is the hand of the left side, and Player 2 is the hand of the right
side. The system uses a countdown timer; when both hands have been successfully classified in
two consecutive frames the result of the round is calculated and the result shown. This mode
checks the strength of the detection pipeline in more complicated scene representations (two
overlapping hand regions).

**E. Game Engine & Statistics Dashboard:** The game engine handles the state of the round,
scoring, and mode changing (single-player vs. two-player). The video feed is overlaid with the
real-time dashboard that shows: current gesture and confidence score, round outcome (Win /
Lose / Draw), total win-loss-draw history, AI insight and confidence on its move, and a move


frequency chart that shows the pattern of behavior of the player. The entire components are
done by the OpenCV drawing functions.

**F. Evaluation:** We will evaluate gesture classification accuracy on a manually recorded test set
of 300 samples (100 per class) collected from 3 participants under varying lighting conditions.
Additional metrics include average frame processing latency (ms), false positive/negative rates
per gesture class, and AI win rate as a function of rounds played (to measure adaptation
effectiveness).

## **3. Project Schedule:**



|Week|Task|Responsible|
|---|---|---|
|1–2 (Mar 25 -–Apr 7)|Literature review; MediaPipe integration;<br>basic hand landmark extraction & visualization|All members|
|3–4 (Apr 8–Apr 21)|Gesture classifier (Rock/Paper/Scissors);<br>confidence score computation & testing|Berrin Aydoğmuş|
|5 (Apr 22-Apr 28)|Adaptive AI module: frequency-based<br>Markov predictor implementation|Tolga Gümüşten|
|6 (Apr 29-May 5)|Two-player mode: dual hand detection,<br>countdown timer, round arbitration|Zohayr Asym|
|7 (May 6–May 12)|Statistics dashboard UI; system integration;<br>full pipeline testing & bug fixing|All members|
|8 (May 13–May 20)|Evaluation (accuracy, latency, AI adaptation);<br>final report writing & presentation prep|All members|

## **References**






[1] F. Zhang, V. Bazarevsky, A. Vakunov, A. Tkachenka, G. Chaurasia, M. T. Chang, and M.
Grundmann, "MediaPipe Hands: On-device Real-time Hand Tracking," _arXiv preprint_
_arXiv:2006.10214_, 2020. [Online]. Available: https://arxiv.org/abs/2006.10214


[2] Google LLC, "MediaPipe Hands Solution Guide," Google Developers, 2024. [Online].
Available: https://developers.google.com/mediapipe/solutions/vision/hand_landmarker


[3] OpenCV Development Team, "OpenCV: Open Source Computer Vision Library," 2024.

[Online]. Available: https://docs.opencv.org


[4] S. Mitra and T. Acharya, "Gesture Recognition: A Survey," _IEEE Transactions on Systems,_
_Man, and Cybernetics, Part C: Applications and Reviews_, vol. 37, no. 3, pp. 311–324, May 2007.

[Online]. Available: https://ieeexplore.ieee.org/document/4154947


[5] V. I. Pavlovic, R. Sharma, and T. S. Huang, "Visual Interpretation of Hand Gestures for
Human-Computer Interaction: A Review," _IEEE Transactions on Pattern Analysis and Machine_
_Intelligence_, vol. 19, no. 7, pp. 677–695, Jul. 1997. [Online]. Available:
https://ieeexplore.ieee.org/document/598228


[6] C. Lugaresi, J. Tang, H. Nash, C. McClanahan, E. Ubowejeke, M. Grundmann, and Y. Song,
"MediaPipe: A Framework for Building Perception Pipelines," _arXiv preprint arXiv:1906.08172_,
2019. [Online]. Available: https://arxiv.org/abs/1906.08172


