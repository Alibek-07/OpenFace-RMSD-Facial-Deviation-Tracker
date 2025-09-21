# OpenFace-RMSD-Facial-Deviation-Tracker
A facial expression deviation tracking system using OpenFace and Root Mean Square Deviation (RMSD) calculations with normalized coordinate systems.

# Overview
This system tracks subtle facial expression changes by:

Establishing a neutral baseline from multiple frames
Using OpenFace for robust 68-point facial landmark detection
Calculating RMSD using normalized face-relative coordinates
Providing real-time visual feedback and data logging
Generating comprehensive analysis reports

Key Features:

- Coordinate normalization (handles position/scale variations)
- Real-time RMSD calculation and visualization
- 3D head pose tracking with visual cube overlay
- Eye gaze direction tracking
- Comprehensive data export (CSV + JSON reports)
- Debug mode with detailed diagnostic information
- Adjustable tracking duration and baseline setup

# Scientific Background
Based on research from Frontiers in Psychology (2020) using the RMSD formula:
RMSD = √(Σ(d(pf, pf+a)²) / N)
Where:

d(pf, pf+a) = Euclidean distance between baseline and current landmark positions
N = 68 facial landmarks
Coordinates are normalized to face-relative space (0-1 range)

Typical RMSD Values:

0.001-0.010: Normal baseline variation
0.010-0.050: Moderate expression changes
0.050+: Significant facial expressions

Prerequisites
System Requirements

Operating System: macOS, Linux, or Windows
Python: 3.7 or higher
Camera: Webcam with decent lighting
RAM: Minimum 4GB (8GB recommended)

Required Software
# OpenFace Installation

# macOS:
#Install dependencies:
- brew install cmake opencv boost dlib

Clone and build OpenFace:
- git clone https://github.com/TadasBaltrusaitis/OpenFace.git
- cd OpenFace
- mkdir build
- cd build
- cmake -D CMAKE_CXX_COMPILER=g++ -D CMAKE_C_COMPILER=gcc -D CMAKE_BUILD_TYPE=RELEASE ..
- make -j4

# Linux (Ubuntu/Debian):
Install dependencies:
- sudo apt-get update
- sudo apt-get install build-essential cmake libopencv-dev libboost-all-dev libdlib-dev

Clone and build OpenFace:
- git clone https://github.com/TadasBaltrusaitis/OpenFace.git
- cd OpenFace
- mkdir build
- cd build
- cmake -D CMAKE_BUILD_TYPE=RELEASE ..
- make -j4


# Python Dependencies
bashpip install opencv-python pandas numpy matplotlib datetime shutil subprocess tempfile json

Installation
Clone this repository:

bashgit clone https://github.com/Alibek-07/OpenFace-RMSD-Facial-Deviation-Tracker.git
cd OpenFace-RMSD-Facial-Deviation-Trackera 

Install Python dependencies:

bashpip install -r requirements.txt

Configure OpenFace path:
Edit the openface_path parameter in the main script:

Update this path to your OpenFace installation:
tracker = RealTimeFacialDeviationTracker(
    openface_path="/path/to/your/OpenFace/installation"
)

# Quick Start:
from facial_deviation_tracker import RealTimeFacialDeviationTracker
import os
import cv2
import numpy as np

#Initialize tracker
tracker = RealTimeFacialDeviationTracker(
    openface_path="/Users/yourusername/openface_install/external_libs/openFace/OpenFace" # Where OpenFAce is located at)

#Step 1: Setup baseline (maintain neutral expression)
print("Setting up baseline...")
if tracker.setup_baseline(num_frames=30):
    print("Baseline established successfully!")
    
    # Step 2: Start real-time tracking
    csv_file, report_file = tracker.start_tracking(duration_minutes=5)   
    
    # Step 3: Analyze results
    tracker.analyze_deviation_patterns()
    
    print(f"Data saved to: {csv_file}")
    print(f"Report saved to: {report_file}")
else:
    print("Failed to establish baseline")

#Cleanup
tracker.cleanup()

# Advanced Adjustment/Configuration:
#Custom baseline setup
tracker.setup_baseline(num_frames=50)  # More frames = more stable baseline

#Extended tracking session
tracker.start_tracking(duration_minutes=10)

#Enable debug mode for troubleshooting
tracker.debug_mode = True

# Instructions
Step 1: Environment Setup
- Ensure good lighting (avoid backlighting)
- Position yourself 60-80cm from the camera
- Have a neutral background if possible
- Close other camera-using applications


Step 2: Baseline Establishment
- Run the script and follow prompts
- Maintain a completely neutral expression during baseline setup
- Stay in the same position and distance
- The system captures 20-30 frames for averaging
- Wait for "Baseline established successfully!" message

Step 3: Real-Time Tracking

The tracking window will open showing:
- Real-time RMSD values
- Facial landmarks overlay
- Status indicators (NORMAL/ELEVATED/HIGH)


Keyboard Controls:

q: Quit tracking
s: Save current data
d: Toggle debug mode


Step 4: Data Analysis

CSV files contain frame-by-frame RMSD data
JSON reports include detailed calculations and statistics
Files are automatically saved to ~/Desktop/Facial Deviation Work/


# Output Files
CSV Data Format:
csvtimestamp,frame,rmsd_deviation,accumulated_rmsd,avg_rmsd,max_landmark_deviation,min_landmark_deviation,std_landmark_deviation,normalization_used
2024-01-01T10:00:00,0,0.023456,0.023456,0.023456,0.045678,0.001234,0.012345,True

JSON Report Structure:
{
  "session_info": {
    "timestamp": "20240101_100000",
    "total_frames": 150,
    "normalization_applied": true
  },
  "summary_statistics": {
    "rmsd_statistics": {
      "mean": 0.034567,
      "median": 0.032145,
      "std": 0.012345
    }
  },
  "detailed_frame_calculations": [...]
}

# Troubleshooting
Common Issues
1. "OpenFace executable not found"
bash# Check if OpenFace is properly installed
ls /path/to/OpenFace/build/bin/FeatureExtraction
or
ls /path/to/OpenFace/exe/FeatureExtraction
2. "No faces detected during baseline"

Improve lighting conditions
Move closer to camera (60-80cm optimal)
Ensure face is clearly visible
Check camera permissions

3. "High RMSD values (>0.1)"

Recalibrate baseline in consistent conditions
Minimize head movement during tracking
Check for detection errors in debug mode

4. Camera access issues
#Test camera access
import cv2
cap = cv2.VideoCapture(0)
if cap.isOpened():
    print("Camera accessible")
    cap.release()
else:
    print("Camera access denied")
Debug Mode
Enable debug mode for detailed diagnostics:
pythontracker.debug_mode = True

Debug output includes:
- Raw vs normalized coordinate comparisons
- Face size consistency checks
- Landmark distance calculations
- Detection quality metrics

# Performance Optimization
Recommended Settings
#For faster processing (lower accuracy)
tracker.setup_baseline(num_frames=15)

#For higher accuracy (slower processing)
tracker.setup_baseline(num_frames=50)

#Adjust camera resolution
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)   # Lower = faster
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)  # Lower = faster


