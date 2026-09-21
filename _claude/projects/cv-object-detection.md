# CV Object Detection — Project Reference
Started: 2026-09-21

## Stack
| Layer | Tool | Notes |
|---|---|---|
| Capture + display | `opencv-python` | VideoCapture, imshow, drawing |
| Detection model | `ultralytics` YOLOv8n | Pretrained COCO (80 classes), ~6MB weights |
| Runtime | Python, CPU | Spare laptop, no discrete GPU |
| UI | OpenCV imshow window | No browser UI for V1 |

```
requirements.txt:
  opencv-python
  ultralytics
```

## Project structure (V1)
```
cv_project/
├── main.py         # loop, orchestration, config constants
├── detector.py     # model loading, inference, result parsing  (milestone 9)
├── display.py      # drawing functions                         (milestone 9)
└── requirements.txt
```
Start with everything in `main.py`. Split in M9.

## Frame flow
```
VideoCapture.read() → frame (NumPy array, BGR, uint8)
  → model.predict(frame) → results
  → parse results[0].boxes (xyxy, conf, cls)
  → cv2.rectangle + cv2.putText on frame
  → cv2.imshow → cv2.waitKey(1)
```

---

## Roadmap

| # | Milestone | Status |
|---|---|---|
| M1 | Webcam open + display feed | ⏳ |
| M2 | Understand frames (shape, dtype, BGR) | ⏳ |
| M3 | FPS counter in frame | ⏳ |
| M4 | Install Ultralytics, load YOLOv8n | ⏳ |
| M5 | Single-frame inference, print raw output | ⏳ |
| M6 | Draw bounding boxes + labels | ⏳ |
| M7 | Connect inference to live loop | ⏳ |
| M8 | Frame skip optimization | ⏳ |
| M9 | Refactor into modules, handle edge cases | ⏳ |

---

## Key Concepts

**Frame shape:** `(height, width, channels)` — e.g. `(480, 640, 3)`
**dtype:** `uint8` — pixel values 0–255
**Color order:** BGR (not RGB). OpenCV always reads Blue first.
**xyxy vs xywh:** xyxy = top-left + bottom-right corners. xywh = center + dimensions. OpenCV drawing wants xyxy.
**Confidence:** float 0–1. `conf=0.4` in predict() = ignore detections below 40%.
**Class ID → name:** `model.names[int(box.cls[0])]`
**verbose=False:** suppresses per-frame console spam in model.predict()

---

## Key Code Patterns

### Main loop skeleton
```python
cap = cv2.VideoCapture(0)  # try 1, 2 if 0 fails
while True:
    ret, frame = cap.read()
    if not ret:
        break
    cv2.imshow("Feed", frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows()
```

### FPS measurement
```python
import time
prev = time.time()
# inside loop:
now = time.time()
fps = 1.0 / (now - prev)
prev = now
cv2.putText(frame, f"FPS: {fps:.1f}", (10, 30),
            cv2.FONT_HERSHEY_SIMPLEX, 1.0, (0, 255, 0), 2)
```

### Model load + inference
```python
from ultralytics import YOLO
model = YOLO("yolov8n.pt")  # load once, before loop
results = model.predict(frame, conf=0.4, verbose=False)
for box in results[0].boxes:
    cls_id = int(box.cls[0])
    label  = model.names[cls_id]
    conf   = float(box.conf[0])
    x1, y1, x2, y2 = map(int, box.xyxy[0])
```

### Draw one detection
```python
color = (0, 255, 0)
cv2.rectangle(frame, (x1, y1), (x2, y2), color, 2)
cv2.putText(frame, f"{label} {conf:.0%}", (x1, y1 - 10),
            cv2.FONT_HERSHEY_SIMPLEX, 0.6, color, 2)
```

### Frame skip
```python
SKIP = 4
frame_count = 0
last_boxes = []
# inside loop:
if frame_count % SKIP == 0:
    results = model.predict(frame, verbose=False)
    last_boxes = results[0].boxes
frame_count += 1
# draw using last_boxes every frame
```

---

## Common Gotchas
- `waitKey` missing → window freezes immediately
- Camera index wrong → try 0, 1, 2; check `cap.isOpened()`
- Coordinates from model are floats → must `map(int, ...)` before drawing
- `y1 - 10` for label can go negative near top of frame → add clamp
- Ultralytics downloads weights on first run → needs internet once
- verbose=True (default) spams the console → always set verbose=False
