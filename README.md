# AI‑Spaghetti Server

Cloud‑deployable AI microservice for **automatic 3D printing failure detection** and **remote printer control**.

---

## Overview

3D printing jobs can fail due to filament tangling or extrusion issues, commonly known as **spaghetti failure**. These failures waste material, damage hardware, and require constant human supervision.

This repository provides a **production‑ready AI microservice** that:

- Detects spaghetti‑type failures using deep learning
- Serves the model through **TorchServe**
- Runs inside a **Docker container with GPU support**
- Sends **pause/stop commands** to a 3D printer controller on a **Raspberry Pi**
- Can be deployed easily on **AWS cloud infrastructure**

The goal is **fully automated monitoring and protection** of long or remote 3D printing jobs.

---

## System Architecture

**Cloud Layer**
- TorchServe hosts the trained detection model
- REST API provides inference and management endpoints
- Docker enables scalable deployment on AWS or other cloud platforms

**Edge Layer**
- Raspberry Pi receives commands from the cloud service
- Interfaces with printer firmware or OctoPrint‑style controller
- Executes real‑time **pause or stop** actions when failure is detected

This design enables a **cloud‑to‑edge autonomous safety loop**.

---

## Repository Structure

```
AI-Spaghetti_Server/
│
├── AI_Model/            # TorchServe model archive (.mar) and configs
├── Dockerfile           # Containerized TorchServe deployment
├── README.md            # Project documentation
└── config.properties    # TorchServe configuration
```

---

## Prerequisites

- Docker with GPU support (NVIDIA Docker runtime)
- CUDA‑compatible GPU
- Python environment for model preparation (optional)
- Network access between cloud service and Raspberry Pi controller

---

## Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/tanmoy-sabir/AI-Spaghetti_Server
cd AI-Spaghetti_Server
```

### 2. Download the TorchServe model archive

```bash
cd AI_Model
wget -c http://~~~~//~~.mar -O solutions_model.mar
cd ..
```

---

## Build Docker Image

```bash
sudo docker build --file Dockerfile -t torchserve_ai:server_ai .
```

---

## Run the Container

```bash
sudo docker run --rm -it \
  --gpus all \
  -p 8080:8080 \
  -p 8081:8081 \
  -p 8082:8082 \
  -p 7070:7070 \
  torchserve_ai:server_ai
```

### Exposed Ports

- **8080** → Inference API
- **8081** → Management API
- **8082** → Metrics API
- **7070** → Custom service communication

---

## API Workflow

1. Camera stream or image frame is sent to the **TorchServe inference endpoint**.
2. AI model predicts **normal vs spaghetti failure**.
3. On failure detection:
   - Cloud service sends **pause/stop command** to Raspberry Pi.
   - Raspberry Pi forwards command to **3D printer controller**.
4. Print job is safely interrupted.

---

## Deployment on AWS

Typical deployment stack:

- **EC2 GPU instance** → runs Docker + TorchServe
- **Security groups** → expose inference API securely
- **Optional S3** → store model archives and logs
- **Optional Lambda / API Gateway** → external trigger or monitoring

This enables **scalable monitoring of multiple printers** from a single AI service.

---

## Use Cases

- Remote 3D printer farms
- Long‑duration industrial prints
- Research labs with unattended printers
- Smart manufacturing monitoring systems

---

## Future Improvements

- Real‑time video stream inference
- Multi‑failure classification beyond spaghetti
- Edge inference directly on Raspberry Pi with lightweight model
- Dashboard for monitoring multiple printers
- Automated job restart after recovery

---

## License

Add your preferred open‑source license here.

---

## Author

**Sabir Hossain**  
Robotics and AI Engineer  

Focused on **autonomous monitoring, cloud robotics, and intelligent manufacturing systems**.
