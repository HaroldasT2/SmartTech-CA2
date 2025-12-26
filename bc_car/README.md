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
