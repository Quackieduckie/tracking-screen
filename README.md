# Tracking Screen

A real-time face tracking and gesture recognition system for Raspberry Pi with pan-tilt camera control.

**Made possible thanks to [PCBWay](https://www.pcbway.com/) for their support in making this project a reality.**

## Features

- **Face Detection & Tracking**: Real-time face detection using MediaPipe
- **Gesture Recognition**: Hand gesture detection (palm for left, shaka for right)
- **Smooth Servo Control**: Pan-tilt camera movement with smooth tracking
- **Background Tracking**: Continues scanning when no face is detected
- **Multiple Detection Methods**: Uses both MediaPipe and OpenCV for robust tracking
- **Customizable Configuration**: YAML-based configuration system

## Hardware Requirements

- Raspberry Pi (4/5 recommended)
- Pan-Tilt camera mount with servos
- Raspberry Pi Camera Module or compatible camera
- Waveshare UGV robot base (optional, for motor control)

## Software Stack

- **Computer Vision**: OpenCV, MediaPipe
- **Camera**: Picamera2, libcamera
- **Hardware Control**: Custom serial communication with ESP32-based controller
- **Processing**: Multi-threaded CV processing for real-time performance

## Installation

```bash
# Clone the repository
git clone https://github.com/Quackieduckie/tracking-screen.git
cd tracking-screen

# Install dependencies
pip3 install -r requirements.txt

# Configure settings
nano config.yaml
```

## Configuration

Edit `config.yaml` to customize:

- Camera resolution and FPS
- Servo limits and speeds
- Detection thresholds
- Gesture recognition settings
- Tracking behavior

## Usage

### Basic Face Tracking

```python
from cv_ctrl import OpencvFuncs
from base_ctrl import BaseController

# Initialize hardware controller
base = BaseController('/dev/ttyAMA0', 115200)

# Initialize CV system
cv = OpencvFuncs('/path/to/project', base)

# Start face tracking
cv.face_detect_switch(True)
cv.base_ctrl.gimbal_base_ctrl(0, 0)  # Center camera
```

### Gesture Recognition

The system recognizes the following gestures:
- **Palm**: Open hand - Move left
- **Shaka**: Hang loose gesture - Move right

### Key Functions

**Face Tracking (`cv_ctrl.py`)**:
- `face_detect_switch(True/False)` - Enable/disable face detection
- `tracking_face_on_switch(True/False)` - Enable/disable camera movement
- `gimbal_base_ctrl(x, y)` - Manual camera positioning

**Hardware Control (`base_ctrl.py`)**:
- `gimbal_ctrl(direction, speed)` - Control servos
- `base_json_ctrl(command)` - Send JSON commands to base
- `pwm_servo_ctrl(id, angle)` - Direct servo control

## Technical Details

### Face Detection Pipeline

1. **Initial Detection**: MediaPipe FaceMesh for fast, accurate face detection
2. **Tracking**: Calculates face center and size
3. **Servo Control**: Smooth movement using PID-like control
4. **Background Scanning**: Pan movement when no face detected
5. **Re-acquisition**: Automatic face search behavior

### Performance Optimizations

- Multi-threaded processing
- Efficient frame buffering
- Adaptive FPS adjustment
- Hardware-accelerated encoding (H.264)
- Smart servo movement limits

### Coordinate System

- **X-axis**: Pan (left-right), range: -45° to +45°
- **Y-axis**: Tilt (up-down), range: -30° to +30°
- Center position: (0, 0)

## Project Structure

```
face-tracking-camera/
├── cv_ctrl.py           # Computer vision and tracking logic
├── base_ctrl.py         # Hardware controller interface
├── config.yaml          # Configuration settings
├── requirements.txt     # Python dependencies
├── models/              # MediaPipe and OpenCV models
├── media/               # Example images and videos
└── README.md           # This file
```

## Dependencies

- Python 3.11+
- OpenCV (cv2)
- MediaPipe
- NumPy
- imutils
- Picamera2
- PyYAML
- pyserial

## Development History

This project evolved from the Waveshare UGV robot platform with extensive custom modifications:

- Enhanced face tracking algorithms
- Added gesture recognition
- Improved servo control smoothness
- Background scanning mode
- Multiple detection method fallbacks
- Performance optimizations for Raspberry Pi 5

## Contributing

Contributions welcome! Areas for improvement:

- Additional gesture recognition
- Multi-face tracking
- Object tracking (beyond faces)
- Better lighting adaptation
- Machine learning-based tracking improvements

## License

Based on Waveshare UGV example code. See LICENSE file for details.

## Credits

- Original UGV platform: Waveshare
- Custom modifications and enhancements: @Quackieduckie
- MediaPipe: Google
- OpenCV: OpenCV Foundation

## Support

For issues or questions, please open an issue on GitHub.

---

**Status**: Active development - tested on Raspberry Pi 5 with custom pan-tilt camera mount
