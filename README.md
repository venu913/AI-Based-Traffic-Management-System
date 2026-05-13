# AI-Based Smart Traffic Management System 

An intelligent, AI-driven traffic management system designed to optimize traffic flow at intersections using real-time video feeds, Computer Vision, and Genetic Algorithms.

## 🗒️ Overview

The Smart Adaptive Traffic Management System leverages advanced AI and computer vision to dynamically control traffic signal timings. Instead of relying on static, pre-programmed timers, this system analyzes live video feeds from a 4-way intersection, counts the number of vehicles in each lane, and processes the data using a Genetic Algorithm to determine the most optimal green light duration for each direction. This minimizes overall traffic delay and prevents congestion.

The project is split into a **React** frontend for user interaction and video uploading, and a **Flask** backend that handles the heavy lifting of AI vehicle detection and algorithmic optimization.

## ✨ Core Features & Technologies

- **Real-Time Vehicle Detection (YOLOv4-tiny)**: The backend utilizes the state-of-the-art YOLOv4-tiny object detection model (via OpenCV DNN module) with CPU/CUDA support to accurately detect and count all types of vehicles (cars, buses, trucks, bikes) across video feeds in real-time. It uses rolling peak-detection to determine the most representative vehicle count over a 30-second window.
- **Genetic Algorithm Traffic Optimization**: Uses a custom Genetic Algorithm implementation (`algo.py`) to minimize total intersection delay. It simulates a population of possible signal timings, applying biological concepts like crossover, mutation, and roulette wheel selection to converge on the optimal green times (between 10s and 60s) for North, South, East, and West directions.
- **Interactive Web Dashboard**: A modern React-based frontend that allows users to seamlessly upload 4 concurrent traffic videos and view the AI-optimized traffic signal recommendations.
- **RESTful API Backend**: A Flask application that exposes endpoints to receive video uploads, coordinate the YOLO detection pipeline, and return the optimized signal timings in JSON format.

## 📁 Project Structure

### Backend (`/backend`)
- `app.py`: The Flask server entry point. Defines the `/upload` API endpoint which receives 4 videos, triggers the detection pipeline, and returns the optimized green light durations.
- `yolov4.py`: Contains the `detect_cars()` function. Loads the YOLOv4-tiny model and weights, processes the video frame-by-frame, and calculates the mean peak vehicle count using `scipy.signal.find_peaks`.
- `algo.py`: Contains the `optimize_traffic()` function and the Genetic Algorithm implementation (`genetic_algorithm`, `crossover`, `mutate`, etc.) to calculate the best green times based on road congestion and capacity.
- `requirements.txt`: Python package dependencies.
- `yolov4-tiny.cfg` & `yolov4-tiny.weights`: The configuration and pre-trained weights for the YOLO model.
- `classes.txt`: Contains the class labels (e.g., "car") that the YOLO model can detect.

### Frontend (`/frontend`)
- `package.json`: Contains the Node.js dependencies and scripts for the React application.
- `src/`: Contains the React components and logic for the user interface.

## 🚀 How to Run

### Prerequisites

Before you begin, ensure you have the following installed on your machine:
- **Python 3.8+**
- **Node.js (v14+) and npm**
- **CUDA Toolkit & cuDNN** (Optional but highly recommended for YOLOv4 GPU acceleration via OpenCV)

### 1. Local Setup (Backend)

Open a terminal and navigate to the `backend` directory to set up the Flask server:

```bash
cd backend

# (Optional) Create a virtual environment
python -m venv venv
# On Windows:
venv\Scripts\activate

# Install the required Python dependencies
pip install -r requirements.txt

# Start the Flask server
python app.py
```
*The backend server will start on `http://127.0.0.1:5000`.*

### 2. Local Setup (Frontend)

Open a **new** terminal and navigate to the `frontend` directory:

```bash
cd frontend

# Install the required Node packages
npm install

# Start the React development server
npm start
```
*The frontend application will open automatically in your browser at `http://localhost:3000`.*

### 3. Using the System
1. Open the web interface in your browser (`http://localhost:3000`).
2. Use the upload area to select and upload **exactly 4 traffic videos** (representing North, South, East, and West directions).
3. The system will send the videos to the Flask backend, where YOLOv4-tiny will analyze the footage to count the vehicles.
4. Once the detection is complete, the Genetic Algorithm will process the car counts to find the optimal signal timings.
5. The interface will display the recommended Green Light Time (in seconds) for each of the 4 directions.

## 🙏 Acknowledgments
- **YOLOv4**: For fast and accurate object detection.
- **OpenCV**: For handling video streams and deep neural network (DNN) operations.
- **React & Flask**: For creating a robust full-stack architecture.
