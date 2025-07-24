# Cross-Camera Player Mapping

## Project Overview

This project aims to **map players across two different video feeds** (`broadcast.mp4` and `tacticam.mp4`) of the same sports game, ensuring each player retains a consistent ID across both camera angles. The solution leverages a fine-tuned YOLOv11 model for player detection and uses appearance-based features to match players between the two views.

---

## Project Setup

### 1. Clone the Repository

```bash
git clone git@github.com:Co-vengers/Cross_Camera_Player_Mapping.git
cd cross_camera_player_mapping
```

### 2. Install Dependencies

Make sure you have Python 3.8+ installed. Then, install the required packages:

```bash
pip install ultralytics opencv-python scipy numpy
```

### 3. Prepare Your Files

- Place your videos in the `videos/` directory:
  - `videos/broadcast.mp4`
  - `videos/tacticam.mp4`
- Place your trained YOLOv11 weights file (e.g., `best.pt`) in the project root.

---

## Approach

1. **Player Detection**  
   - Each frame from both videos is processed using the YOLOv11 model to detect players.
   - Only detections with high confidence and class label `2` (player) are considered.

2. **Feature Extraction**  
   - For each detected player, the corresponding image patch is cropped and resized.
   - A color histogram (3D, 8 bins per channel) is computed for each player crop, capturing the distribution of colors as an appearance feature.

3. **Player Matching**  
   - For each frame, the color histograms of detected players from both videos are compared.
   - Cosine distance is used to measure similarity between histograms.
   - Each player in the broadcast view is matched to the most similar player in the tacticam view (minimum cosine distance).

---

## Challenges

- **Appearance Similarity:**  
  Players may have similar uniforms or lighting conditions may vary, making color-based matching less robust in some cases.

- **Occlusions and Missed Detections:**  
  Players may be occluded or missed by the detector in one view but not the other, leading to incomplete or incorrect mappings.

- **Synchronization:**  
  Ensuring that frames from both videos are temporally aligned is crucial for accurate matching.

- **Scalability:**  
  The approach is currently frame-based and does not use temporal tracking, which could improve robustness over time.

---

## How to Run

1. Ensure your videos and model weights are in place.
2. Run the main script or notebook:

```bash
python yolo.py
# or
jupyter notebook yolo.ipynb
```

3. The output will display the player mapping between the two camera views for the first frame.

---

## Future Improvements

- Integrate player tracking across frames for more robust and consistent ID assignment.
- Incorporate additional features (e.g., spatial location, jersey number recognition) for improved matching.
- Enhance synchronization between video feeds.

---

## Acknowledgements

- [Ultralytics YOLO](https://github.com/ultralytics/ultralytics)
- OpenCV, SciPy, NumPy
