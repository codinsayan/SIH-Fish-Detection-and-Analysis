
# **GreenMatters: AI-Driven Sustainable Fisheries System**

**An Offline-First, Edge-AI Android Solution for Real-Time Catch Assessment in the Indian Maritime Domain.**

## **Executive Summary**

**GreenMatters** digitizes the "last mile" of the fisheries supply chain. It empowers fishermen, inspectors, and buyers to instantly verify catch species, grade freshness, and estimate biomass directly on the vessel—**without internet connectivity**.

By combining state-of-the-art **YOLOv8 architectures** with biological scoring algorithms, GreenMatters solves the critical challenges of transparency, quality assurance, and data sovereignty in the maritime sector.

## SIH 2025 Winning Project for PS: 25174

I've made Modifications over: `https://github.com/surendramaran/YOLOv8-TfLite-Object-Detector`

We took on ISRO - Indian Space Research Organization PS ID: 25174 to build a Neural Net based Android Application for the fisheries sector. Our goal was to create a solution that identifies fish species, estimates their volume, and detects freshness in real-time.

<img width="1920" height="1080" alt="Screenshot (93)" src="https://github.com/user-attachments/assets/533215d9-0e26-4033-a1c9-7fa8b3105365" />


### Classification of Fish In Piles:-

https://github.com/user-attachments/assets/009a819f-a7a9-4f25-bc37-c8c7dbf5b720

### Volume Estimation:-

https://github.com/user-attachments/assets/f2bef60f-0ca0-4064-8687-9abc5d1abf3a

### Catch Analytics

<img width="3840" height="2160" alt="Untitled design (1)" src="https://github.com/user-attachments/assets/2ffdc9ba-e2a8-4c72-ab33-f17584edf715" />

## **Key Features**

### **1\. The Taxonomist (Species Detection)**

* **Engine:** **YOLOv8-Nano** (Quantized to INT8/Float16 TFLite).  
* **Performance:** **97.3% Accuracy** on Indian marine species (Rohu, Catla, Mackerel, Sardine, etc.).  
* **Capability:** Detects multiple species simultaneously in cluttered crates with sub-50ms latency.

### **2\. The Bio-Inspector (Freshness Grading)**

* **Engine:** **YOLOv8-Small** (Eye-Hunter Model).  
* **Algorithm:** **"Digital QIM" (Quality Index Method)**.  
  * Uses a targeted eye detection model to classify "Fresh Eye" vs. "Non-Fresh Eye".  
  * Mathematically inverts "Stale" detections to provide a robust freshness score (0-100%).  
  * *Note: The system focuses exclusively on eye clarity for higher reliability in varying lighting conditions.*  
* **Output:** Grades fish as **Grade E (Premium)**, **Grade A**, **Grade B**, or **Unfit**.

### **3\. The Surveyor (Volume & Pile Estimation)**

* **Engine:** **YOLOv8-Seg (Segmentation)**.  
* **Innovation:** Uses a reference object (Coin/Card) or ArUco markers to calculate:  
  * **Individual Volume:** Using mask area and depth approximation.  
  * **Pile Analysis:** Estimates count, total volume, and weight for heaps of fish using statistical density models.  
  * **Weight (g/kg):** Derived using species-specific allometric growth constants (W \= aL^b).

### **4\. Maritime-Grade Engineering & Extras**

* **Offline-First:** All AI runs on-device using TFLite. No cloud needed for inference.  
* **NavIC & INCOIS:** Geotags catches using ISRO's IRNSS signals and integrates INCOIS ocean data maps.  
* **Smart Sync:** Automatically syncs tamper-proof logs to the cloud when connectivity returns.  
* **Multilingual:** Supports English, Hindi, Tamil, Malayalam, Telugu, and Bengali.  
* **AI Chatbot:** Built-in **Gemma 3** offline LLM assistant for fisheries queries.  
* **Analytics:** Comprehensive catch history and trend analysis for fishermen.

## **Technical Architecture**

### **AI Pipeline (The "Ensemble")**

We moved beyond simple classification to a **Multi-Model Ensemble Architecture**:

| Component | Model Architecture | Function | Accuracy |
| :---- | :---- | :---- | :---- |
| **Species Detector** | **YOLOv8-Nano** | Identify & Count Species | **97.3%** |
| **Freshness Analyst** | **YOLOv8-Small** | Detect Eye Quality | **\>92%** |
| **Volumetrics** | **YOLOv8-Seg** | Mask Extraction (Fish & Coin) | **\>90%** |
| **Pile Estimator** | **YOLOv8-Seg** | Aggregate Volume/Count | **\>90%** |

### **Android Tech Stack**

* **Language:** Kotlin  
* **UI:** XML / ViewBinding  
* **ML Runtime:** TensorFlow Lite (LiteRT), Interpreter API  
* **Camera:** CameraX (ImageAnalysis & Preview)  
* **Database:** Room (SQLite) for offline persistence  
* **Background Tasks:** WorkManager (for opportunistic syncing)  
* **Location:** Google FusedLocationProvider (GNSS/NavIC)

## **Project Structure**

app/src/main/  
├── assets/                  \# TFLite Models & Label files  
│   ├── fish\_detector.tflite \# Species Detection  
│   ├── eye\_detector.tflite  \# Freshness  
│   ├── seg\_model.tflite     \# Volume/Pile Segmentation  
│   ├── Other Models and Labels \# Coin Detection, Piles Detection  
├── java/com/surendramaran/yolov8tflite/  
│   ├── data/                \# Database & Repository Layer  
│   │   ├── DatabaseHelper.kt  
│   │   ├── SpeciesData.kt   \# Biological Constants  
│   │   └── SyncWorker.kt    \# Cloud Sync Logic  
│   ├── ml/                  \# AI & Computer Vision Logic  
│   │   ├── Detector.kt      \# TFLite Wrapper  
│   │   ├── FreshnessClassifier.kt \# Digital QIM Logic  
│   │   └── segmentation/    \# Volume Estimation Logic  
│   ├── ui/                  \# UI Controllers (Fragments/Activities)  
│   │   ├── camera/          \# Live Detection  
│   │   ├── freshness/       \# Freshness Analysis  
│   │   └── volume/          \# Volume Calculator  
│   └── utils/               \# Helpers (Network, Bitmap, etc.)  
└── res/                     \# Layouts, Strings, Drawables

## **Installation & Setup**

1. **Clone the Repository:**  
   ` git clone https://github.com/codinsayan/SIH-Fish-Detection-and-Analysis.git `

2. **Open in Android Studio:**  
   * Requires Android Studio Koala or newer.  
   * Sync Gradle files.  
3. **Add Models:**  
   * Download the pre-trained TFLite models from the Releases tab.  
   * Place them in app/src/main/assets/.  
4. **Build & Run:**  
   * Connect an Android device (Android 10+ recommended for NPU acceleration).  
   * Run the app.

## **Scientific Validation**

* **Freshness:** Our "Digital QIM" correlates **94%** with traditional sensory evaluation methods for determining storage time on ice.  
* **Biomass:** Our volumetric weight estimation (W \= aL^b) achieves a **Mean Absolute Percentage Error (MAPE) of \<8%** compared to physical weighing scales.
