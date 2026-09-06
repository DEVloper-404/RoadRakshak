# RR2 (RoadRakshak) Project Overview

## 1. Tech Stack
* **Dashboard:** Node.js (Backend), HTML/CSS/JS (Frontend)
* **AI Model:** Python
* **Cloud Processing:** Google Colab (GPU)
* **Video Feed:** Webcam7 + Secure Tunneling

## 2. File & Folder Structure

### `/dashboard` (The UI & Server)
* `server.js`: Node.js server that runs the dashboard and handles API requests.
* `public/`: Stores frontend UI files (HTML, CSS, JS) that users see.
* `models/`: Database schemas for organizing dashboard data.
* `package.json`: Manages the Node.js dependencies.

### `/model` (The AI Engine)
* `process_video.py`: Python script for reading and analyzing video feeds.
* `requirements.txt`: List of Python dependencies (like OpenCV, PyTorch).

## 3. Webcam7 & Secure Tunneling
* **What it is:** We use secure tunneling (like ngrok or Cloudflare Tunnels) to expose the local Webcam7 stream.
* **Why it's needed:** It provides a safe, encrypted URL so Google Colab can access your local camera feed over the internet without compromising your local network.

## 4. Cloud AI & Dashboard Workflow
1. **AI on Colab:** To prevent slowing down local machines, the heavy detection model runs on a Google Colab notebook to use free GPUs.
2. **API Endpoint:** Colab processes the secure Webcam7 video feed in real-time and serves an API endpoint.
3. **Dashboard Fetch:** The Node.js dashboard (`server.js`) calls the Colab API to get the latest JSON data.
4. **Display:** The UI (`public/`) dynamically updates to showcase the detections.

## 5. JSON Data Format
The Colab API will return the processed detection data in this crisp, simple JSON format:

```json
{
  "timestamp": "2026-09-06T12:45:00Z",
  "camera_id": "webcam7_tunnel",
  "status": "active",
  "detections": [
    {
      "type": "pothole",
      "confidence": 0.92,
      "bounding_box": [120, 50, 200, 150]
    },
    {
      "type": "vehicle",
      "confidence": 0.88,
      "bounding_box": [300, 150, 450, 300]
    }
  ]
}
```
