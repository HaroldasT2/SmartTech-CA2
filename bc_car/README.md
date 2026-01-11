# Self-Driving Car Smart Tech CA 2

This project implements autonomous driving using deep machine learning.

## Installation

### Required Packages

Install all required packages using the following commands:

```bash
# Core packages for bc.py
pip install numpy<2.0
pip install opencv-python<4.10
pip install matplotlib tensorflow pandas Pillow scikit-learn imgaug

# Packages for drive.py (specific versions required for simulator compatibility)
pip install flask
pip install python-socketio==4.6.0 python-engineio==3.13.2
pip install eventlet
```

### Package Details

**For bc.py (Training):**
- `numpy<2.0` - NumPy (downgraded for imgaug compatibility)
- `opencv-python<4.10` - OpenCV (downgraded for NumPy 1.x compatibility)
- `matplotlib` - Plotting and visualization
- `tensorflow` - Deep learning framework with Keras
- `pandas` - Data manipulation
- `Pillow` - Image processing
- `scikit-learn` - Train/test split and data utilities
- `imgaug` - Image augmentation

**For drive.py (Autonomous Mode):**
- `flask` - Web framework
- `python-socketio==4.6.0` - WebSocket communication (specific version for simulator)
- `python-engineio==3.13.2` - Engine.IO protocol (specific version for simulator)
- `eventlet` - WSGI server

## Usage

### Training the Model

Run the training script to create a new model:

```bash
python bc.py
```

### Running Autonomous Mode

1. **Start the Python server:**
   ```bash
   python drive.py
   ```
   Wait until you see: `wsgi starting up on http://0.0.0.0:4567` (or something similar to that)

2. **Launch the simulator:**
   - Navigate to the unity game folder (either version 1 or 2), the download link is https://github.com/udacity/self-driving-car-sim
   - Run the simulator executable

3. **Connect to autonomous mode:**
   - Select one of the tracks
   - Choose **"Autonomous Mode"**
   - The simulator will connect to `localhost:4567`

4. **Watch the car drive:**
   - The trained model will control steering and throttle
   - The car should drive autonomously around the track

### Troubleshooting

- Ensure `python drive.py` is running **before** starting autonomous mode in the simulator
- Make sure the model file `nvidia_elu_augmented.keras` exists in the bc_car directory
- If the car doesn't move, check that you're using the correct socketio versions (4.6.0 and 3.13.2)
- Try Version 1 simulator (`term1-simulator-windows`) if Version 2 has issues

## Recording Training Data

### Prerequisites
1. Download the Udacity Self-Driving Car Simulator: 
   - Visit: https://github.com/udacity/self-driving-car-sim
   - Download Version 2 (includes Rocky Mountain map)
   - Extract to a convenient location

### Setting Up Data Recording

1. **Configure Save Location:**
   - Launch the simulator
   - Go to Settings (if available)
   - Set output directory to `bc_car/DataForCar/` in your project folder
   - If no settings option, the simulator will save to its default location and you'll need to move the files

2. **Understanding the Data Structure:**
   After recording, your data will be organized as:
   ```
   DataForCar/
   ├── driving_log.csv          # Contains steering angles, throttle, speed, brake
   └── IMG/                      # Camera images folder
       ├── center_[timestamp].jpg
       ├── left_[timestamp].jpg
       └── right_[timestamp].jpg
   ```

### Recording Process

#### Basic Recording Steps:
1. **Launch the simulator**
2. **Select a track:**
   - Track 1 (Lake Track) - Simpler, good for initial training
   - Track 2 (Rocky Mountain) - More challenging, better generalization
3. **Choose "Training Mode"**
4. **Press the RECORD button** (or press 'R' key)
5. **Drive manually:**
   - Use Arrow Keys or WASD for steering/acceleration
   - Drive smoothly and stay centered in the lane
   - Brake before corners, accelerate on straightaways
6. **Press RECORD again** to stop recording
7. **Data automatically saves** to your configured directory

#### What the Simulator Captures:
- **Images**: Three camera views (center, left, right) for each frame
- **Steering angle**: Your steering input (-1.0 to 1.0)
- **Throttle**:  Acceleration input (0.0 to 1.0)
- **Speed**: Current vehicle speed
- **Brake/Reverse**: Additional control inputs

### Recording Strategy for Best Results

#### 1. **Track 1 - Base Data (2-3 laps)**
- Drive smoothly in the center of the lane
- Maintain consistent speed
- This provides the foundation of your training data

#### 2. **Recovery Data (Multiple short clips)**
- Position car near the edge of the road
- Start recording
- Steer smoothly back to the center
- Stop recording
- Repeat this process at different locations
- **Why:** Teaches the model how to recover from mistakes

#### 3. **Corner-Focused Data (Additional laps)**
- Most of the track is straight, creating imbalanced data
- Record extra laps focusing on cornering
- Drive slowly and smoothly through corners
- **Why:** Ensures model has enough examples of turning

#### 4. **Track 2 - Generalization Data (2-3 laps)**
- After training on Track 1, record data from Track 2
- The Rocky Mountain map has different scenery and challenges
- **Why:** Tests if the model can generalize to new environments

#### 5. **Additional Tips:**
- **Smooth driving**:  Avoid jerky movements - the model will learn from your driving style
- **Variety**: Record at different times with different lighting if possible
- **Balanced steering**: Try to get equal amounts of left/right turns
- **Volume**:  Aim for 5,000-10,000 images minimum for good results

### After Recording

1. **Verify your data:**
   ```bash
   # Check that driving_log.csv exists
   ls DataForCar/driving_log.csv
   
   # Check number of images recorded
   ls DataForCar/IMG/ | wc -l
   ```

2. **Inspect driving_log.csv:**
   - Open in Excel or text editor
   - Should have columns: center, left, right, steering, throttle, brake, speed
   - Each row represents one frame of recorded data

3. **Ready to train:**
   Once you have sufficient data recorded, you can train the model:
   ```bash
   python bc.py
   ```

### Troubleshooting Data Recording

| Issue | Solution |
|-------|----------|
| Can't find recorded data | Check simulator settings for output path, or look in simulator's default directory |
| No IMG folder created | Make sure you pressed RECORD before driving |
| Too few images | Record more laps. Aim for at least 5,000 images |
| Car drives poorly in autonomous mode | Need more balanced data. Add recovery and corner data |
| Model works on Track 1 but not Track 2 | Record additional data from Track 2 |
