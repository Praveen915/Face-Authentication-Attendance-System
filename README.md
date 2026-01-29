SmartAttendance: Computer Vision Based Attendance System

A lightweight, Python-based automated attendance solution utilizing computer vision techniques. This project implements a custom facial recognition pipeline using **OpenCV** and **NumPy**, designed to run efficiently on CPU without the overhead of heavy Deep Learning frameworks.

## 🚀 Project Overview

This system replaces manual attendance processes with a contactless, biometric approach. It features a custom-built face encoding algorithm that converts facial features into 128-dimensional vectors, enabling real-time matching using Euclidean distance metrics. It also includes basic liveness detection (blink tracking) to prevent static photo spoofing.

### Key Capabilities

* **Real-time Processing:** Low-latency detection using Haar Cascade Classifiers.
* **Biometric Verification:** Custom 128-D vector encoding (Histogram + Spatial features).
* **Liveness Check:** Integrated blink detection to ensure the user is present.
* **Data Persistence:** Lightweight JSON and binary (NumPy) storage for portability.
* **Session Management:** Built-in cooldown logic to prevent duplicate entries.

---

## 🛠️ Technical Architecture

### 1. Detection Layer

Instead of using heavy CNNs (like MTCNN), this project utilizes **Haar Cascades**.

* **Why:** Zero dependencies, extremely fast inference (30+ FPS on standard CPUs), and ideal for controlled lighting environments.

### 2. Feature Extraction (Encoding)

A heuristic approach to generating a unique face signature:

* **Global Features:** 64-bin Histogram of Oriented Gradients (Grayscale intensity).
* **Local Features:** 8x8 spatial grid averaging to capture structural geometry.
* **Result:** A compact 128-dimensional vector representing the face.

### 3. Matching Logic

* **Algorithm:** Euclidean Distance ().
* **Threshold:** Configurable sensitivity (Default: 8.0).
* **Logic:** If the distance between the live face and stored vector is below the threshold, access is granted.

---

## 📂 Project Structure

```text
face-auth-attendance/
├── app.py                  # Main execution script
├── camera/                 # Webcam stream handling
├── face/
│   ├── detector.py         # Haar Cascade implementation
│   ├── encoder.py          # Vector generation logic
│   └── matcher.py          # Euclidean distance calculator
├── attendance/
│   ├── attendance.py       # Core business logic
│   └── storage.py          # Data IO (JSON/NPY)
├── spoof/                  # Anti-spoofing modules (Blink)
├── utils/                  # Configuration & Helper functions
└── data/                   # Database (Auto-generated)

```

---

## ⚙️ Installation & Setup

**Prerequisites:** Python 3.11+, Webcam.

**1. Clone the repository**

```bash
git clone https://github.com/shivam1342/face-auth-attendance.git
cd face-auth-attendance

```

**2. Initialize Environment**

```bash
python -m venv venv
# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

```

**3. Install Dependencies**

```bash
pip install -r requirements.txt

```

**4. Launch System**

```bash
python app.py

```

---

## 🎮 Controls & Usage

The system operates via a terminal-based interface hooked into the video feed.

| Key | Function | Description |
| --- | --- | --- |
| **`r`** | **Register** | Captures 7 frames to build a robust face profile. |
| **`i`** | **In-Time** | Punches in. Requires a natural blink for liveness verification. |
| **`o`** | **Out-Time** | Punches out to close the attendance session. |
| **`s`** | **Summary** | Prints the current day's logs to the console. |
| **`l`** | **List** | Shows all registered user IDs. |
| **`q`** | **Quit** | Terminates the program safely. |

**Usage Tips:**

1. **Registration:** Ensure good lighting. When you press `r`, input your name in the terminal, then look at the camera until the green success flash.
2. **Liveness:** During punch-in (`i`), the system will wait for you to blink naturally to prove you are not a photograph.

---

## ⚠️ Limitations & Future Scope

While effective for academic and demonstration purposes, this system works within specific constraints:

* **Lighting Sensitivity:** The custom encoder relies on pixel intensity, so performance may degrade in very low light.
* **Liveness Security:** The current blink detection is a heuristic method. For high-security production environments, active IR depth sensing would be required.
* **Scalability:** Currently uses file-based storage (JSON). Future iterations would implement SQL/NoSQL databases for handling thousands of users.

## 🔧 Configuration

You can fine-tune the system sensitivity in `utils/config.py`:

* `FACE_MATCH_THRESHOLD`: Decrease this value for stricter matching (less false positives).
* `LIVENESS_TIMEOUT_SECONDS`: Adjust the time window allowed for a blink.

---

## 👨‍💻 Author

**Praveen Kumar Mishra**

* Final Year B.Tech CSE Student
* Project Focus: Computer Vision & Algorithmic Optimization

## License

MIT License
