# Biometric Auth — Face Recognition Roadmap
Created: 2026-08-08
Status: Planning / Learning

---

## Decision: Fingerprint vs Face Recognition

### Fingerprint Scanner — honest assessment
You cannot "build" a fingerprint scanner. The biometric processing lives inside the hardware and OS — you never get raw fingerprint data, by design (security). What you *can* do:

- **WebAuthn / Passkeys** — Web standard. If the device has a fingerprint sensor (phone, laptop), the browser exposes it via `navigator.credentials.create()`. You get a cryptographic credential back, not a fingerprint image. Essentially you're calling an API, not building anything.
- **Windows Hello** — Same model, Windows-only via Edge/Chrome.

**Learning value: Low.** You write 20 lines of JS and the OS does everything. Nothing to understand about biometrics.

### Face Recognition — genuinely buildable
Pure software. You can understand and implement every layer:
1. Detect faces in an image (bounding box)
2. Align & normalize the face
3. Convert it to a numeric embedding (vector)
4. Compare embeddings via cosine similarity
5. Classify: known person or unknown?

**Learning value: High.** Real ML pipeline. RTX 5060 Ti 16GB is perfect for it.

**Recommendation: Build face recognition.**

---

## How Face Recognition Works (Mental Model)

```
Camera frame
    ↓
[Detection] — "Is there a face? Where?"
    ↓
[Alignment] — Normalize eyes/nose/mouth to fixed positions
    ↓
[Embedding] — Neural net converts face → 512-float vector
    ↓
[Comparison] — Cosine similarity vs known face vectors
    ↓
[Decision] — Match (>threshold) → identity, else → unknown
```

Two completely separate problems:
- **Detection**: Finding faces in an image (OpenCV, MTCNN)
- **Recognition**: Identifying *whose* face it is (dlib, ArcFace, InsightFace)

---

## Library Landscape

| Library | Backend | GPU | Difficulty | Best for |
|---|---|---|---|---|
| `opencv-python` | C++ | Optional | Easy | Detection only, learning fundamentals |
| `face_recognition` | dlib | CPU only | Easy | Learning full pipeline, prototyping |
| `DeepFace` | TF/Keras | Yes | Medium | Multi-model comparison, fast results |
| `InsightFace` | ONNX | Yes (CUDA) | Medium | Production, best accuracy, fast on RTX |

**Progression: OpenCV → face_recognition → InsightFace**

---

## Roadmap

### Phase 0 — Theory (1–2 days)
Goal: Understand the pipeline before writing code.

- [ ] Read: how Haar cascades work (classic detection)
- [ ] Read: how CNN-based detection works (MTCNN, RetinaFace)
- [ ] Understand: what a face embedding is (512-dim vector, not a photo)
- [ ] Understand: cosine similarity (distance between vectors)
- [ ] Watch: 1 YouTube video on ArcFace or FaceNet architecture

Resources:
- [face-recognition GitHub](https://github.com/ageitgey/face_recognition) — read the README fully
- [InsightFace GitHub](https://github.com/deepinsight/insightface)
- OpenCV docs on Haar cascades

---

### Phase 1 — Face Detection (2–3 days)
Goal: Webcam → draw bounding boxes around faces in real time.

Stack: `opencv-python`, Python

```python
import cv2

face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5)
    for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
    cv2.imshow('Face Detection', frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
```

Milestones:
- [ ] Webcam opens and displays feed
- [ ] Faces get a green rectangle in real time
- [ ] Works at 30fps without lag

---

### Phase 2 — Face Recognition (3–5 days)
Goal: Identify specific people from a known-faces folder.

Stack: `face_recognition` (pip install face-recognition)

Concepts to implement:
- Build a known-faces database (photos folder, one per person)
- Load each photo → compute embedding → store with name label
- For each webcam frame: detect → embed → compare → display name

Milestones:
- [ ] Enroll 3+ known faces from photos
- [ ] Webcam identifies each person by name
- [ ] Unknown faces labelled "Unknown"
- [ ] Confidence score displayed

Note: `face_recognition` runs CPU-only. Acceptable for learning, not production speed.

---

### Phase 3 — GPU-Accelerated with InsightFace (5–7 days)
Goal: Replace the CPU pipeline with a fast ONNX model running on RTX 5060 Ti.

Stack: `insightface`, `onnxruntime-gpu`, CUDA

Setup steps:
1. Install CUDA Toolkit (match your driver version)
2. `pip install insightface onnxruntime-gpu`
3. InsightFace downloads models automatically on first run
4. Use `buffalo_l` model (best accuracy/speed balance)

Milestones:
- [ ] InsightFace running on GPU (verify with `nvidia-smi` during inference)
- [ ] Real-time recognition at 60fps+
- [ ] Embeddings stored in a JSON or SQLite DB
- [ ] Add/remove faces without restarting

---

### Phase 4 — FastAPI Integration (3–5 days)
Goal: Expose recognition as an API endpoint.

Stack: FastAPI, InsightFace, Pillow

```
POST /recognize
Body: { image: base64 }
Response: { matches: [{ name, confidence }], unknown: bool }

POST /enroll
Body: { name, image: base64 }
Response: { enrolled: true }
```

Milestones:
- [ ] `/recognize` endpoint returns identity from a base64 image
- [ ] `/enroll` stores a new face embedding
- [ ] Proper error handling (no face detected, multiple faces, etc.)

---

### Phase 5 — Web Frontend (3–5 days)
Goal: Browser webcam → face recognized → result displayed.

Stack: Vanilla JS or React, fetch API

Flow:
1. Access webcam via `getUserMedia`
2. Capture a frame to canvas → `canvas.toDataURL()` → base64
3. POST to FastAPI `/recognize`
4. Display result overlay

Milestones:
- [ ] Webcam feed renders in browser
- [ ] "Identify" button captures frame and POSTs it
- [ ] Identity or "Unknown" displayed below the feed
- [ ] Enroll new face from browser

---

### Phase 6 — Hardening (optional, ongoing)
- [ ] Liveness detection (blink test, head turn) to prevent photo spoofing
- [ ] Rate limiting on API
- [ ] Persistent embedding store (SQLite or Supabase)
- [ ] Multiple face support (identify multiple people in one frame)

---

## Environment Setup

```bash
# Core
pip install opencv-python face_recognition

# GPU pipeline (after Phase 2)
pip install insightface onnxruntime-gpu

# API
pip install fastapi uvicorn pillow python-multipart
```

RTX 5060 Ti 16GB — more than enough VRAM for buffalo_l model (~1GB).

---

## Key Concepts to Keep Straight

| Term | Meaning |
|---|---|
| Detection | Finding face locations in an image |
| Recognition | Identifying *who* the face belongs to |
| Embedding | 512-number vector representing a face |
| Cosine similarity | Distance metric between embeddings (0–1, higher = more similar) |
| Threshold | Minimum similarity score to call it a match (usually 0.4–0.6) |
| Liveness detection | Checking the face is real (not a photo/video) |

---

## Open Questions
- Which use case? Auth gate vs person labelling vs something else?
- Real-time video or snapshot-based?
- Where does the known-faces DB live? Local file, SQLite, Supabase?
- Eventually integrate with VALE or standalone tool?
