# 🎭 Camouflage — Video & Image Steganography Tool

## Complete Project Documentation

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [System Requirements](#2-system-requirements)
3. [Installation & Setup Guide](#3-installation--setup-guide)
4. [Project Architecture](#4-project-architecture)
5. [How the Project Works (SOP)](#5-how-the-project-works-sop)
6. [Technical Deep Dive](#6-technical-deep-dive)
7. [URL Routes & Pages](#7-url-routes--pages)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Project Overview

**Camouflage** is a web-based steganography tool built with Django that allows users to hide secret text messages inside video and image files. The hidden messages are invisible to the naked eye — the video/image looks completely normal to anyone viewing it.

### Use Cases

- **Secure Communication**: Two users can exchange videos containing hidden messages, appearing as normal video sharing to any third party.
- **Copyright Protection**: Embed invisible watermark text into YouTube videos to prove ownership.
- **Confidential Data Transfer**: Transmit sensitive text data disguised within media files.

### Key Features

| Feature              | Description                                              |
| -------------------- | -------------------------------------------------------- |
| **Video Encoding**   | Hide a secret message inside an AVI video file           |
| **Video Decoding**   | Extract the hidden message from an encoded AVI video     |
| **Image Encoding**   | Hide a secret message inside a PNG/JPG/JPEG image        |
| **Image Decoding**   | Extract the hidden message from an encoded image         |
| **RC4 Encryption**   | Messages are encrypted with a secret key before encoding |
| **Web Interface**    | Easy-to-use browser-based UI built with Bootstrap 5      |

---

## 2. System Requirements

### Software Requirements

| Software      | Version         | Purpose                           |
| ------------- | --------------- | --------------------------------- |
| Python        | 3.10+           | Programming language              |
| pip           | 22.0+           | Python package manager            |
| Git           | Any recent      | Cloning the repository            |
| Web Browser   | Any modern      | Accessing the application         |

### Hardware Requirements

| Resource | Minimum            | Recommended          |
| -------- | ------------------ | -------------------- |
| RAM      | 4 GB               | 8 GB+                |
| Storage  | 500 MB (+ videos)  | 2 GB+                |
| CPU      | Dual Core          | Quad Core            |

> **Note**: Video encoding/decoding is CPU and memory intensive. Larger videos require more resources and processing time.

### Python Dependencies

The project requires the following Python packages (defined in `requirements.txt`):

| Package              | Version  | Purpose                                |
| -------------------- | -------- | -------------------------------------- |
| Django               | 4.2.5    | Web framework                          |
| opencv-python        | 4.8.1.78 | Video frame reading/writing            |
| numpy                | 1.26.2   | Array manipulation for pixel data      |
| moviepy              | 1.0.3    | Video processing and audio handling    |
| Pillow               | 10.0.1   | Image processing for image steg        |
| imageio              | 2.33.1   | Image I/O operations                   |
| imageio-ffmpeg       | 0.4.9    | FFmpeg backend for video processing    |
| crispy-bootstrap4    | 2023.1   | Bootstrap 4 form rendering             |
| django-crispy-forms  | 2.1      | Better Django form layouts             |
| django-widget-tweaks | 1.5.0    | Custom form field rendering            |
| django-bootstrap-v5  | 1.0.11   | Bootstrap 5 integration                |
| whitenoise           | 6.6.0    | Static file serving                    |
| psycopg2-binary      | 2.9.9    | PostgreSQL adapter (optional)          |
| python-dotenv        | 1.0.0    | Environment variable management        |
| beautifulsoup4       | 4.12.2   | HTML parsing                           |
| requests             | 2.31.0   | HTTP library                           |

---

## 3. Installation & Setup Guide

### Step 1: Clone the Repository

```bash
git clone https://github.com/whyme-duh/video-steganography.git
cd video-steganography
```

### Step 2: Install pip (if not already installed)

```bash
# Ubuntu/Debian
sudo apt install python3-pip

# Verify installation
pip3 --version
```

### Step 3: Fix the requirements.txt Encoding (Important!)

> ⚠️ **Known Issue**: The original `requirements.txt` is encoded in UTF-16LE format, which pip cannot read. You must convert it to UTF-8 first.

```bash
# Check the encoding
file requirements.txt
# If it shows "UTF-16", convert it:
iconv -f UTF-16LE -t UTF-8 requirements.txt | sed 's/\r$//' > requirements_fixed.txt
mv requirements_fixed.txt requirements.txt
```

### Step 4: Install Python Dependencies

```bash
pip3 install -r requirements.txt
```

Verify Django is installed:

```bash
python3 -c "import django; print(django.VERSION)"
# Expected output: (4, 2, 5, 'final', 0) or similar
```

### Step 5: Create Required Media Directories

The application needs specific folders inside the `media/` directory to store uploaded, encoded, and decoded files.

```bash
mkdir -p media/encoded/image_encode
mkdir -p media/decoded
mkdir -p media/videos
mkdir -p media/images
mkdir -p media/icons
```

Your directory structure should look like:

```
media/
├── decoded/
├── encoded/
│   └── image_encode/
├── images/
├── icons/
└── videos/
```

### Step 6: Run Database Migrations

Django uses a database (SQLite by default) to manage its internal data:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

Expected output:

```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  ...
  Applying sessions.0001_initial... OK
```

### Step 7: Run the Development Server

```bash
python3 manage.py runserver
```

Expected output:

```
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

### Step 8: Access the Application

Open your web browser and navigate to:

```
http://127.0.0.1:8000/
```

You should see the Camouflage home page with options for encoding and decoding.

---

## 4. Project Architecture

### Directory Structure

```
video-steganography/
│
├── manage.py                  # Django management script
├── requirements.txt           # Python dependencies
├── runtime.txt                # Python version for deployment (Heroku)
├── Procfile                   # Deployment config for Heroku
├── db.sqlite3                 # SQLite database (auto-created)
│
├── camouflage/                # Django project settings
│   ├── __init__.py
│   ├── settings.py            # Project configuration
│   ├── urls.py                # Root URL routing
│   ├── wsgi.py                # WSGI entry point
│   └── asgi.py                # ASGI entry point
│
├── core/                      # Video steganography app
│   ├── __init__.py
│   ├── admin.py               # Admin panel registration
│   ├── apps.py                # App configuration
│   ├── forms.py               # EncodeForm & DecodeForm definitions
│   ├── models.py              # Database models (commented out)
│   ├── views.py               # View functions (home, encode, decode, etc.)
│   ├── main.py                # *** Core steganography logic ***
│   ├── urls.py                # App-level URL routing
│   ├── RC4/                   # RC4 encryption module
│   │   ├── __init__.py
│   │   └── rc4.py             # RC4 cipher implementation
│   └── templates/core/
│       ├── index.html         # Base template (navbar, scripts, CSS)
│       ├── home.html          # Landing/home page
│       ├── encode.html        # Video encoding form page
│       ├── decode.html        # Video decoding form & result page
│       ├── sucess.html        # Encoding success/result page
│       ├── about.html         # About page
│       └── 404.html           # Error page
│
├── imageSteg/                 # Image steganography app
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py               # ImageEncodeForm & DecodeForm
│   ├── models.py              # ImageEncoding model
│   ├── views.py               # Image encode/decode logic
│   ├── urls.py                # Image steg URL routes
│   └── templates/imageSteg/
│       ├── imageEncode.html   # Image encoding form
│       ├── imageDecode.html   # Image decoding form & result
│       └── success.html       # Image encoding success page
│
├── static/                    # Static files (CSS, JS, images)
│   ├── style.css              # Main stylesheet
│   ├── index.js               # JavaScript for UI interactions
│   └── admin/                 # Django admin static files
│
└── media/                     # User-uploaded & processed files
    ├── encoded/               # Encoded video output
    │   └── image_encode/      # Encoded image output
    ├── decoded/               # Decoded outputs
    ├── videos/                # Uploaded videos
    ├── images/                # Uploaded images
    └── icons/                 # Application icons
```

### Architecture Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                        USER (Browser)                        │
│                    http://127.0.0.1:8000                      │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│                     Django Web Server                         │
│                    (manage.py runserver)                      │
├────────────────────────┬─────────────────────────────────────┤
│                        │                                     │
│    camouflage/urls.py  │   Root URL Router                   │
│         │              │   ├── /          → core.urls        │
│         │              │   ├── /image/    → imageSteg.urls   │
│         │              │   └── /admin/    → Django admin     │
│         ▼              │                                     │
├────────────────────────┼─────────────────────────────────────┤
│                        │                                     │
│  ┌─────────────────┐   │   ┌──────────────────────┐          │
│  │   core (App)    │   │   │  imageSteg (App)     │          │
│  │                 │   │   │                      │          │
│  │  views.py       │   │   │  views.py            │          │
│  │  ├── home()     │   │   │  ├── encode_request()│          │
│  │  ├── encode()   │   │   │  ├── decode_req()    │          │
│  │  ├── decode()   │   │   │  └── image_sucess()  │          │
│  │  ├── sucess()   │   │   │                      │          │
│  │  └── about_us() │   │   │  forms.py            │          │
│  │                 │   │   │  models.py            │          │
│  │  forms.py       │   │   └──────────────────────┘          │
│  │  main.py ◄──────│───│──── Core Steganography Engine      │
│  │  RC4/rc4.py ◄───│───│──── Encryption Module              │
│  └─────────────────┘   │                                     │
│                        │                                     │
├────────────────────────┼─────────────────────────────────────┤
│                        │                                     │
│  Templates (HTML)      │   Static Files                      │
│  ├── index.html        │   ├── style.css                     │
│  ├── home.html         │   ├── index.js                      │
│  ├── encode.html       │   └── images (bg.png, etc.)         │
│  ├── decode.html       │                                     │
│  └── sucess.html       │                                     │
│                        │                                     │
├────────────────────────┼─────────────────────────────────────┤
│                        │                                     │
│  Database (SQLite)     │   Media Storage                     │
│  └── db.sqlite3        │   ├── encoded/                      │
│                        │   ├── decoded/                      │
│                        │   ├── videos/                       │
│                        │   └── images/                       │
└────────────────────────┴─────────────────────────────────────┘
```

### Technology Stack

| Layer          | Technology                                |
| -------------- | ----------------------------------------- |
| Frontend       | HTML5, CSS3, JavaScript, Bootstrap 5      |
| Backend        | Python 3.10, Django 4.2.5                 |
| Database       | SQLite3 (default) / PostgreSQL (optional) |
| Video Process  | OpenCV, MoviePy, FFmpeg                   |
| Image Process  | Pillow (PIL)                              |
| Encryption     | Custom RC4 Cipher Implementation          |
| Deployment     | Gunicorn, WhiteNoise (Heroku-ready)       |

---

## 5. How the Project Works (SOP)

### Standard Operating Procedure

---

### SOP 1: Video Encoding (Hiding a Message in a Video)

#### Purpose

To embed a secret text message inside a video file so that the video appears unchanged to viewers but contains a hidden message retrievable only with the correct secret key.

#### Prerequisites

- The application must be running (`python3 manage.py runserver`)
- You must have an **AVI format** video file ready
- The message must be **50 characters or less**
- The secret key must be **less than 10 characters**

#### Step-by-Step Procedure

| Step | Action                                                                            | Expected Result                              |
| ---- | --------------------------------------------------------------------------------- | -------------------------------------------- |
| 1    | Open browser and go to `http://127.0.0.1:8000/`                                  | Home page loads                              |
| 2    | Click **"Encode Video"** button on the home page OR navigate to **Encode → Video Encoding** in the navbar | Video encoding form page opens               |
| 3    | Click **"Choose File"** and select your `.AVI` video file                         | File name appears next to the button         |
| 4    | Enter a **Secret Key** (max 10 characters, e.g., `mykey123`)                      | Key is entered in the field                  |
| 5    | Enter your **Secret Message** (max 50 characters, e.g., `Hello from the other side`) | Message is entered                           |
| 6    | Enter a **File Name** for the encoded output (no extension, e.g., `encoded_video`) | Filename is entered                          |
| 7    | Click the **"Encode"** button                                                     | Loading spinner appears while processing     |
| 8    | Wait for encoding to complete                                                     | Redirected to success page                   |
| 9    | On the success page, view completion time and file sizes                           | Shows original vs encoded video size         |
| 10   | Click **"Download Video"** to save the encoded video                              | Encoded `.avi` file downloads to your system |

#### What Happens Behind the Scenes

```
User Input → Form Validation → Video Upload → Frame Extraction
    → RC4 Encryption of Message → LSB Encoding into Frames
    → Video Reconstruction with Audio → Output Encoded Video
```

1. The uploaded video is saved temporarily in the `media/` folder
2. OpenCV reads every frame of the video into a NumPy array
3. MoviePy extracts the audio track separately
4. The secret message is encrypted using the RC4 cipher with the provided key
5. The encrypted binary data is embedded into the Least Significant Bits (LSB) of video frame pixels
6. The first frame stores metadata (message length + frame index) in its LSB
7. The modified frames are reassembled into a video with the original audio
8. The encoded video is saved as an AVI file in `media/encoded/`
9. The temporary uploaded file is deleted

---

### SOP 2: Video Decoding (Extracting a Hidden Message from a Video)

#### Purpose

To extract and read a hidden text message from a previously encoded video file using the correct secret key.

#### Prerequisites

- An encoded `.AVI` video file (produced by the encoding step)
- The **same secret key** that was used during encoding

#### Step-by-Step Procedure

| Step | Action                                                                              | Expected Result                                   |
| ---- | ----------------------------------------------------------------------------------- | ------------------------------------------------- |
| 1    | Open browser and go to `http://127.0.0.1:8000/`                                    | Home page loads                                   |
| 2    | Navigate to **Decode → Video Decoding** in the navbar                               | Video decoding form page opens                    |
| 3    | Click **"Choose File"** and select the encoded `.AVI` video file                    | File name appears                                 |
| 4    | Enter the **Secret Key** (must match the one used during encoding)                   | Key is entered                                    |
| 5    | Click the **"Decode"** button                                                       | Loading spinner appears while processing          |
| 6    | Wait for decoding to complete                                                       | A card popup appears with the result              |
| 7a   | **If successful**: The decoded message is displayed on screen                        | Shows "Decoded Successfully" with the message     |
| 7b   | **If failed**: An error message is shown                                             | Shows "Error" — wrong key or non-encoded video    |
| 8    | (Optional) Click **"Export in Text"** to download the decoded message as a `.txt` file | `decode.txt` file downloads                      |

#### What Happens Behind the Scenes

```
User Input → Form Validation → Video Upload → Frame Extraction
    → Read Metadata from Frame 0 → Extract Encrypted Bits from Frames
    → RC4 Decryption → Display Plaintext Message
```

1. The uploaded encoded video is saved temporarily
2. OpenCV reads all frames into memory
3. The first frame's LSB data is read to extract:
   - **Message length** (first 48 bits)
   - **Random index** (next 48 bits) — the frame where data starts
4. Using the random index, the algorithm reads LSBs from the correct frames
5. The extracted binary data is decrypted using RC4 with the provided secret key
6. The resulting plaintext is validated (checked for printable ASCII characters)
7. If valid, the decoded message is displayed; otherwise, an error is shown
8. The decoded message is also saved to `media/decode.txt`
9. The temporary uploaded file is cleaned up

---

### SOP 3: Image Encoding (Hiding a Message in an Image)

#### Purpose

To embed a secret text message inside an image file (PNG/JPG/JPEG).

#### Step-by-Step Procedure

| Step | Action                                                              | Expected Result                          |
| ---- | ------------------------------------------------------------------- | ---------------------------------------- |
| 1    | Navigate to **Encode → Image Encoding** in the navbar               | Image encoding form page opens           |
| 2    | Click **"Choose File"** and select a PNG/JPG/JPEG image             | File name appears                        |
| 3    | Enter your **Secret Message** (max 80 characters)                   | Message is entered                       |
| 4    | Click the **"Encode"** / Submit button                              | Image is processed                       |
| 5    | Redirected to success page with completion time                     | Shows encoding results                   |

#### What Happens Behind the Scenes

1. The image is loaded using Pillow (PIL)
2. The message is converted to binary (8 bits per character)
3. Pixel RGB values are modified at the LSB level to store the binary message
4. The 9th pixel in each group signals whether more data follows (odd LSB = end, even = continue)
5. The encoded image is saved as `media/encoded/image_encode/encoded.png`

---

### SOP 4: Image Decoding (Extracting a Hidden Message from an Image)

#### Purpose

To extract and read a hidden text message from a previously encoded image.

#### Step-by-Step Procedure

| Step | Action                                                              | Expected Result                          |
| ---- | ------------------------------------------------------------------- | ---------------------------------------- |
| 1    | Navigate to **Decode → Image Decoding** in the navbar               | Image decoding form page opens           |
| 2    | Click **"Choose File"** and select the encoded PNG/JPG/JPEG image   | File name appears                        |
| 3    | Click the **"Decode"** button                                       | Image is processed                       |
| 4    | The decoded message is displayed on the page                        | Shows decoded text or error              |

#### What Happens Behind the Scenes

1. The image is loaded pixel by pixel using Pillow
2. LSBs are read from groups of 3 pixels (9 values total per character)
3. The first 8 LSBs form one character (8 bits = 1 byte = 1 ASCII character)
4. The 9th value's LSB acts as a stop flag (odd = last character reached)
5. Characters are assembled until the stop signal is encountered
6. The decoded message is saved to `media/image_decode.txt`

---

## 6. Technical Deep Dive

### 6.1 LSB (Least Significant Bit) Steganography

The core technique used is **LSB Steganography** — modifying the least significant bit of pixel color values to store hidden data.

#### How It Works (Conceptual Example)

```
Original pixel RGB value:  (142, 203, 87)
In binary:                 (10001110, 11001011, 01010111)
                                   ^         ^         ^
                            LSB ───┘   LSB ──┘   LSB ─┘

To hide the bits "1 0 1":
Modified pixel:            (10001111, 11001010, 01010111)
                                   ^         ^         ^
Result RGB value:          (143, 202, 87)
```

The change is invisible — RGB value only shifts by ±1, which is imperceptible to the human eye.

#### Why Videos Are Good for Steganography

- A single 1920×1080 video frame has **1,920 × 1,080 × 3 = 6,220,800 bits** available for LSB storage
- That's roughly **760 KB** of hidden data per frame
- A 30-second video at 30fps has **900 frames** — massive storage capacity
- Video compression artifacts mask the tiny LSB changes even further

### 6.2 RC4 Encryption

Before embedding into the video, messages are encrypted using the **RC4 (Rivest Cipher 4)** stream cipher for additional security.

#### RC4 Algorithm Steps

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Secret Key │ ──► │ Key Scheduling   │ ──► │ Permutation     │
│  (user)     │     │ Algorithm (KSA)  │     │ Array S[256]    │
└─────────────┘     └──────────────────┘     └────────┬────────┘
                                                      │
                                                      ▼
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Plaintext  │ ──► │ Pseudo-Random    │ ──► │  Ciphertext     │
│  Message    │     │ Generation (PRGA)│     │  (binary bits)  │
└─────────────┘     │ XOR with keystream│     └─────────────────┘
                    └──────────────────┘
```

1. **KSA (Key Scheduling Algorithm)**: Creates a 256-byte permutation array `S` based on the secret key
2. **PRGA (Pseudo-Random Generation Algorithm)**: Generates a key stream by further permuting `S`
3. **Encryption**: Each character of the message is XORed with the key stream to produce 8-bit binary ciphertext
4. **Decryption**: The same process in reverse — XOR ciphertext with the same key stream to get plaintext

> **Security Note**: The same secret key must be used for both encryption and decryption. Without the correct key, the output will be unintelligible.

### 6.3 Video Encoding Algorithm (Detailed)

```python
# Simplified flow of the Video.encode() method:

1. Read all video frames into a NumPy array
2. Calculate total available storage in pixel LSBs
3. Check if message fits in available space
4. Pad message to align with frame boundaries
5. Convert message length to 48-bit binary
6. Encrypt message using RC4 → produces binary ciphertext
7. Reshape ciphertext into frame-sized arrays
8. Pick a random starting frame index (between 1 and end)
9. XOR ciphertext with LSBs of target frames
10. Store metadata in Frame 0:
    - Bits 0-47:  Message length (in characters)
    - Bits 48-95: Random frame index (where data starts)
11. Reassemble frames → write video with original audio
```

#### Frame 0 — Metadata Storage

```
Frame 0 LSB Layout:
┌────────────────────────┬────────────────────────┬──────────────────┐
│  Message Length (48b)  │  Start Frame Index(48b)│  Unused LSBs     │
│  Bits 0 — 47           │  Bits 48 — 95          │  96+             │
└────────────────────────┴────────────────────────┴──────────────────┘
```

### 6.4 Video Decoding Algorithm (Detailed)

```python
# Simplified flow of the Video.decode() method:

1. Read all video frames into a NumPy array
2. Extract LSBs from Frame 0
3. Read bits 0-47  → message length
4. Read bits 48-95 → random start index
5. Calculate how many frames contain data
6. Extract LSBs from frames[start_index : start_index + N]
7. Assemble extracted bits into ciphertext string
8. Decrypt ciphertext using RC4 with the secret key
9. Return plaintext message
```

### 6.5 Image Steganography Algorithm

The image steganography uses a simpler LSB approach without encryption:

#### Encoding

```
For each character in the message:
  1. Take 3 pixels (9 RGB values)
  2. Convert character to 8-bit binary
  3. Set LSBs of first 8 values to match the 8 bits
  4. Set 9th value's LSB:
     - Even (0) = more characters follow
     - Odd  (1) = this is the last character
```

#### Decoding

```
Loop through pixels in groups of 3:
  1. Read LSBs of first 8 values → 8-bit binary → 1 character
  2. Check 9th value's LSB:
     - If odd  → stop, return accumulated message
     - If even → continue to next group
```

---

## 7. URL Routes & Pages

### Core App Routes (`/`)

| URL Path     | View Function | Page Description                      |
| ------------ | ------------- | ------------------------------------- |
| `/`          | `home()`      | Landing page with project introduction |
| `/encode/`   | `encode()`    | Video encoding form                   |
| `/decode/`   | `decode()`    | Video decoding form & results         |
| `/success/`  | `sucess()`    | Encoding success page with download   |
| `/aboutus/`  | `about_us()`  | About page                            |

### Image Steganography App Routes (`/image/`)

| URL Path              | View Function      | Page Description                    |
| --------------------- | ------------------ | ----------------------------------- |
| `/image/steg/`        | `encode_request()` | Image encoding form                 |
| `/image/decode-image/`| `decode_req()`     | Image decoding form & results       |
| `/image/image-steg-success/` | `image_sucess()` | Image encoding success page    |

### Navigation Structure

```
Navbar
├── Home          →  /
├── Encode (dropdown)
│   ├── Video Encoding  →  /encode/
│   └── Image Encoding  →  /image/steg/
├── Decode (dropdown)
│   ├── Video Decoding  →  /decode/
│   └── Image Decoding  →  /image/decode-image/
└── About         →  /aboutus/
```

---

## 8. Troubleshooting

### Common Issues & Solutions

#### ❌ `ModuleNotFoundError: No module named 'django'`

**Cause**: `requirements.txt` was encoded in UTF-16LE, so pip couldn't read it properly.

**Fix**:

```bash
# Convert requirements.txt to UTF-8
iconv -f UTF-16LE -t UTF-8 requirements.txt | sed 's/\r$//' > requirements_fixed.txt
mv requirements_fixed.txt requirements.txt

# Reinstall dependencies
pip3 install -r requirements.txt
```

#### ❌ `FileNotFoundError` for media directories

**Cause**: Required media folders don't exist.

**Fix**:

```bash
mkdir -p media/encoded/image_encode media/decoded media/videos media/images media/icons
```

#### ❌ `SECRET_KEY` is `None` warning

**Cause**: The `SECRET_KEY` is loaded from an environment variable that isn't set.

**Fix**: Create a `.env` file in the project root:

```bash
echo "SECRET_KEY=your-random-secret-key-here-make-it-long" > .env
```

Or generate one:

```bash
python3 -c "from django.core.management.utils import get_random_secret_key; print(f'SECRET_KEY={get_random_secret_key()}')" > .env
```

#### ❌ Video encoding fails or is very slow

**Cause**: Video processing is resource-intensive.

**Fix**:
- Use shorter, smaller resolution videos
- Ensure you have enough RAM (8GB+ recommended)
- Only `.AVI` format is supported

#### ❌ Decoded message shows garbage text

**Cause**: Wrong secret key or the video was not encoded by this tool.

**Fix**:
- Double-check the secret key matches exactly
- Ensure you're using the encoded video file (not the original)

#### ❌ Image encoding gives a Windows path error

**Cause**: The `imageSteg/views.py` contains a hardcoded Windows file path.

**Fix**: This path in `imageSteg/views.py` should be updated to use relative paths compatible with your OS. The hardcoded path `C:/Users/ritik_yxb9lpe/...` needs to be replaced with a path relative to the project directory.

---

## Quick Reference Card

```
┌──────────────────────────────────────────────────────┐
│              CAMOUFLAGE — Quick Reference             │
├──────────────────────────────────────────────────────┤
│                                                      │
│  START SERVER:  python3 manage.py runserver           │
│  ACCESS:        http://127.0.0.1:8000/               │
│                                                      │
│  VIDEO ENCODE:  /encode/                             │
│    • Format: .AVI only                               │
│    • Secret Key: max 10 chars                        │
│    • Message: max 50 chars                           │
│    • Output: media/encoded/<filename>.avi            │
│                                                      │
│  VIDEO DECODE:  /decode/                             │
│    • Input: Encoded .AVI file                        │
│    • Requires: Same secret key used in encoding      │
│    • Output: Displayed on screen + decode.txt        │
│                                                      │
│  IMAGE ENCODE:  /image/steg/                         │
│    • Format: PNG, JPG, JPEG                          │
│    • Message: max 80 chars                           │
│    • Output: media/encoded/image_encode/encoded.png  │
│                                                      │
│  IMAGE DECODE:  /image/decode-image/                 │
│    • Input: Encoded PNG/JPG/JPEG                     │
│    • No key needed                                   │
│    • Output: Displayed on screen + image_decode.txt  │
│                                                      │
│  TECH:  LSB Steganography + RC4 Encryption (video)   │
│         LSB Steganography only (image)               │
│                                                      │
└──────────────────────────────────────────────────────┘
```

---

*Documentation prepared for the Camouflage Video Steganography Project*
