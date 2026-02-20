# MedSter-Hack-21 — Smart Healthcare Management System

> :bomb: Entry for the **Hack 2021** hackathon by HackerEarth.

---

## Overview

**MedSter** is a desktop-based Smart Healthcare Management System that connects **Doctors**, **Patients**, and **Pharmacists** through a unified PyQt5 GUI front-end backed by a Django REST API. Doctors are authenticated via **real-time face recognition** (powered by OpenCV), while patients and pharmacists use traditional credential-based login. The system supports appointment scheduling, patient record management, and data export.

---

## Key Features

- **Multi-role GUI** – Separate login portals for Doctors, Patients, and Pharmacists, all launched from a single main window.
- **Face Recognition Login for Doctors** – Uses OpenCV's LBPH (Local Binary Patterns Histograms) face recognizer; new doctors are enrolled by capturing 100 face samples via webcam.
- **Doctor Registration** – New doctors are registered by capturing face images, training the face recognizer, and storing profile pictures locally.
- **Appointment Management** – REST API endpoints (Django) to create, retrieve, and manage patient appointments.
- **Data Export** – Each user role can export their relevant data directly from the GUI.
- **Dark-themed UI** – Built with PyQt5 and the QDarkStyle theme for a modern look.

---

## Tech Stack

| Layer | Technology |
|---|---|
| GUI | Python 3, PyQt5, QDarkStyle |
| Face Recognition | OpenCV (`cv2`), LBPH Face Recognizer, Haar Cascades |
| Backend API | Django 3, Django REST Framework |
| Data Storage | SQLite (Django ORM), CSV files |
| Image Processing | OpenCV, Pillow, NumPy |

---

## Project Structure

```
MedSter-Hack-21/
├── gui.py                          # Main window – entry point for the GUI
├── doctor.py                       # Doctor portal UI (Login / Sign-up / Export)
├── patient.py                      # Patient portal UI (Login / Sign-up / Export)
├── pharma.py                       # Pharmacist portal UI (Login / Sign-up / Export)
├── doctor_login.py                 # Face-recognition-based doctor authentication
├── doctor_sign.py                  # New doctor enrolment (face capture + training)
├── patient_login.py                # Patient login (in development)
├── patient_sign.py                 # Patient registration (in development)
├── appointments.py                 # Example script for the Appointments REST API
├── HACK_apis.zip                   # Django REST API source (extract before use)
├── haarcascade_frontalface_default.xml  # OpenCV Haar Cascade for face detection
├── trainer/
│   └── trainer.yml                 # Trained LBPH face recognizer model
├── Doctors.csv                     # Registered doctor names
├── id.csv                          # Auto-incrementing doctor ID counter
└── requirements.text               # Python dependency list
```

---

## Prerequisites

- Python 3.7+
- A webcam (required for face recognition features)
- `pip` package manager

---

## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/akulka404/MedSter-Hack-21.git
   cd MedSter-Hack-21
   ```

2. **Extract the Django API**

   ```bash
   unzip HACK_apis.zip
   ```

3. **Install Python dependencies**

   ```bash
   pip install -r requirements.text
   ```

4. **Apply Django database migrations** (from inside the extracted API folder)

   ```bash
   python3 manage.py migrate
   ```

---

## Running the Application

### 1. Start the Django REST API server

```bash
python3 manage.py runserver
```

The API will be available at `http://localhost:8000/`.

### 2. Launch the GUI

```bash
python3 gui.py
```

The main window presents three buttons: **Doctor Login**, **Pharmacist Login**, and **Patient Login**.

---

## Usage Guide

### Doctor Sign-up (Face Enrolment)

1. Open the GUI → click **Doctor Login** → click **Doctor Sign-up**.
2. Enter the new doctor's name when prompted in the terminal.
3. The webcam opens and captures **100 face samples** automatically.
4. The LBPH face recognizer is re-trained and saved to `trainer/trainer.yml`.
5. A profile picture is saved to `profile_pictures/<name>.jpg`.

### Doctor Login (Face Recognition)

1. Open the GUI → click **Doctor Login** → click **Doctor Login**.
2. The webcam activates and scans for a recognised face.
3. On successful recognition, the doctor's dashboard opens automatically.

### Appointments API

The REST API exposes an `/appointments/` endpoint. Example usage:

```python
import requests

# List all appointments
response = requests.get("http://localhost:8000/appointments/")
print(response.json())

# Create a new appointment
data = {
    "patient_name": "Jane Doe",
    "gender": "F",
    "address": "123 Main St",
    "number": 1,
    "appointment_date": "2021-02-10",
    "time": "10:00 - 11:00",
    "doctor_name": "Dr. Smith",
    "hospital_name": "City Hospital",
    "description": "Routine check-up"
}
resp = requests.post("http://localhost:8000/appointments/", json=data)
print(resp.status_code)
```

---

## Notes

- Patient login and patient sign-up flows are currently **in development** (placeholder implementations).
- Pharmacist login/sign-up flows are also **in development**.
- The face recognizer model (`trainer/trainer.yml`) must be present before running doctor login. Run **Doctor Sign-up** at least once to generate it.
- The `dataset/` and `profile_pictures/` directories are created automatically during doctor enrolment; ensure write permissions exist in the project folder.

