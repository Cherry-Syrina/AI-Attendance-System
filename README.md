<div align="center">

# 📸 SnapClass

### AI-Powered Attendance System — Take Attendance in Seconds with Face & Voice Recognition!

[![Python](https://img.shields.io/badge/Python-3.9+-blue?style=for-the-badge&logo=python)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-App-FF4B4B?style=for-the-badge&logo=streamlit)](https://streamlit.io)
[![Supabase](https://img.shields.io/badge/Supabase-Database-3ECF8E?style=for-the-badge&logo=supabase)](https://supabase.com)

</div>

---

## 🚀 About the Project

**SnapClass** is an AI-powered smart attendance system that eliminates the hassle of manual attendance tracking. Simply upload a class photo or record audio — the system automatically identifies each student and marks their attendance.

> 💡 No more calling out names. Just snap a photo and you're done!

---

## ✨ Features

| Feature | Description |
|---|---|
| 📷 **Face Recognition** | Automatically detect and identify all students from a single class photo |
| 🎙️ **Voice Recognition** | Identify students by their voice from an audio recording |
| 👨‍🏫 **Teacher Portal** | Create subjects, enroll students, and view attendance records |
| 👨‍🎓 **Student Portal** | View your attendance history and enrolled subjects |
| 🔗 **Auto Enroll via QR / Link** | Students can join a subject instantly using a shared join code |
| 📊 **Attendance Analytics** | Subject-wise and student-wise attendance reports |
| 🔐 **Secure Authentication** | Teacher login and registration with bcrypt password hashing |

---

## 🛠️ Tech Stack

- **Frontend:** [Streamlit](https://streamlit.io)
- **Database:** [Supabase](https://supabase.com) (PostgreSQL)
- **Face Recognition:** `dlib` + `face_recognition_models` + `scikit-learn` (SVM Classifier)
- **Voice Recognition:** `resemblyzer` + `librosa`
- **Authentication:** `bcrypt`
- **QR Code Generation:** `segno`
- **Image Processing:** `Pillow`, `numpy`

---

## 📁 Project Structure

```
ai-attendance-project-app/
│
├── app.py                              # Main entry point
├── requirements.txt                    # Project dependencies
│
└── src/
    ├── components/                     # Reusable UI dialog components
    │   ├── dialog_add_photo.py             # Face enrollment dialog
    │   ├── dialog_attendance_results.py    # Attendance results view
    │   ├── dialog_auto_enroll.py           # Auto-enroll via QR/link
    │   ├── dialog_create_subject.py        # Create new subject
    │   ├── dialog_enroll.py                # Manual student enrollment
    │   ├── dialog_share_subject.py         # Share subject / generate QR
    │   ├── dialog_voice_attendance.py      # Voice attendance dialog
    │   ├── footer.py
    │   └── header.py
    │
    ├── database/
    │   ├── config.py                   # Supabase connection config
    │   └── db.py                       # All database operations
    │
    ├── pipelines/
    │   ├── face_pipeline.py            # Face detection & SVM classifier
    │   └── voice_pipeline.py           # Voice embedding & speaker identification
    │
    ├── screens/
    │   ├── home_screen.py              # Landing page (Teacher / Student selection)
    │   ├── teacher_screen.py           # Teacher dashboard
    │   └── student_screen.py           # Student dashboard
    │
    └── ui/
        └── base_layout.py              # Shared styles and layout
```

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/ai-attendance-project-app.git
cd ai-attendance-project-app
```

### 2. Create a Virtual Environment (Recommended)

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac / Linux
source venv/bin/activate
```

### 3. Install Dependencies

> ⚠️ `dlib` requires CMake and C++ build tools to be installed on your system before running pip install.

```bash
pip install -r requirements.txt
```

### 4. Set Up Supabase

1. Create a free account at [supabase.com](https://supabase.com)
2. Create a new project
3. Run the following SQL in the Supabase SQL Editor to set up the required tables:

```sql
-- Teachers Table
create table teachers (
  id serial primary key,
  username text unique not null,
  password text not null,
  name text not null
);

-- Students Table
create table students (
  student_id serial primary key,
  name text not null,
  face_embedding jsonb,
  voice_embedding jsonb
);

-- Subjects Table
create table subjects (
  id serial primary key,
  subject_code text unique not null,
  name text not null,
  section text,
  teacher_id int references teachers(id)
);

-- Subject-Student Enrollment
create table subject_students (
  id serial primary key,
  student_id int references students(student_id),
  subject_id int references subjects(id)
);

-- Attendance Logs
create table attendance_logs (
  id serial primary key,
  student_id int references students(student_id),
  subject_id int references subjects(id),
  timestamp timestamptz default now()
);
```

### 5. Configure Environment Variables

Add your Supabase credentials to `src/database/config.py`:

```python
SUPABASE_URL = "https://your-project.supabase.co"
SUPABASE_KEY = "your-anon-key"
```

### 6. Run the App

```bash
streamlit run app.py
```

The app will open in your browser at `http://localhost:8501` ✅

---

## 🎯 Usage Guide

### 👨‍🏫 For Teachers

1. Select **Teacher Portal** from the home screen
2. Register a new account or log in
3. Create a new subject using **"Create Subject"**
4. Share the **QR code or join link** with your students
5. When it's time to take attendance:
   - 📸 **Face Attendance:** Upload a class photo — the system will identify all students automatically
   - 🎙️ **Voice Attendance:** Record an audio clip — students are identified by their voice

### 👨‍🎓 For Students

1. Select **Student Portal** from the home screen
2. Enter your name and log in
3. Use the teacher's **join code** to enroll in a subject
4. View your **attendance history** and **enrolled subjects**
5. Register your **face photo** or **voice sample** for recognition

---

## 🤖 How the AI Works

### Face Recognition Pipeline
```
Class Photo → dlib Face Detector → 128-dim Face Embedding → SVM Classifier → Student ID
```
- **Threshold:** Euclidean distance ≤ 0.6 — faces beyond this threshold are marked as unknown

### Voice Recognition Pipeline
```
Audio Recording → librosa Segmentation → Resemblyzer Embeddings → Cosine Similarity → Speaker ID
```
- **Threshold:** Cosine similarity ≥ 0.65 — below this, no match is returned

---

## 🔮 Planned Improvements

- [ ] Better handling of multiple faces in a single frame
- [ ] Real-time camera feed support
- [ ] Email / push notifications for attendance
- [ ] Export attendance reports to Excel or PDF
- [ ] Mobile app version

---

## 🤝 Contributing

Contributions are welcome! Please open an issue first to discuss what you'd like to change.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

<div align="center">

Live Demo:https://attendance-snap-ai.streamlit.app/

Made with ❤️ by Sushma Shukla

*Smarter classrooms start here.*

</div>
