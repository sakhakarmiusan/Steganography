# 🎭 CAMOUFLAGE — Complete Project Guide

## For External Presentation & Understanding

---

## Table of Contents

1. [What Is This Project?](#1-what-is-this-project)
2. [Technologies Used (Full List)](#2-technologies-used-full-list)
3. [Project Folder Structure — Explained](#3-project-folder-structure--explained)
4. [How All Files Connect To Each Other](#4-how-all-files-connect-to-each-other)
5. [Django Framework — How It Works Here](#5-django-framework--how-it-works-here)
6. [Module-by-Module Breakdown](#6-module-by-module-breakdown)
7. [The Core Algorithm — LSB Steganography](#7-the-core-algorithm--lsb-steganography)
8. [RC4 Encryption — How It Works](#8-rc4-encryption--how-it-works)
9. [Complete Code Walkthrough](#9-complete-code-walkthrough)
10. [Request-Response Flow (What Happens When User Clicks)](#10-request-response-flow-what-happens-when-user-clicks)
11. [Frontend — Templates & Styling](#11-frontend--templates--styling)
12. [Database & Media Storage](#12-database--media-storage)
13. [Deployment Configuration](#13-deployment-configuration)
14. [Python Libraries & Why Each Is Needed](#14-python-libraries--why-each-is-needed)
15. [Common Questions For Presentation](#15-common-questions-for-presentation)

---

## 1. What Is This Project?

**Camouflage** is a web application that lets users **hide secret text messages inside video and image files**. This technique is called **Steganography** — the practice of concealing a message within another medium (like a video or image) so that no one except the intended recipient even knows a message exists.

### In Simple Words:

- You upload a **video** or **image**
- You type a **secret message** and a **secret key** (password)
- The app **hides that message** inside the video/image pixels
- The video/image **looks completely normal** to anyone who sees it
- Only someone with the **same secret key** can extract and read the hidden message

### Real-World Use Cases:

| Use Case | Example |
|----------|---------|
| **Secure Communication** | Two people exchange videos with hidden messages — looks like normal video sharing |
| **Copyright Protection** | Embed an invisible watermark text in your YouTube videos to prove ownership |
| **Confidential Data Transfer** | Send sensitive information disguised as a normal image/video |
| **Military/Intelligence** | Covert communication channels |

---

## 2. Technologies Used (Full List)

### Backend Technologies

| Technology | Version | What It Does |
|-----------|---------|--------------|
| **Python** | 3.10.4 | The main programming language for all backend logic |
| **Django** | 4.2.5 | Web framework — handles URLs, views, forms, templates, database |
| **OpenCV (cv2)** | 4.8.1.78 | Reads and writes video frames as arrays of pixels |
| **NumPy** | 1.26.2 | Fast mathematical operations on pixel arrays |
| **MoviePy** | 1.0.3 | Handles video audio extraction and merging |
| **Pillow (PIL)** | 10.0.1 | Image processing — reads/writes image pixels |
| **FFmpeg** (via imageio-ffmpeg) | 0.4.9 | Video codec backend — encodes/decodes video formats |

### Frontend Technologies

| Technology | Version | What It Does |
|-----------|---------|--------------|
| **HTML5** | — | Page structure and content |
| **CSS3** | — | Custom styling, animations, responsive design |
| **JavaScript** | ES6 | Loading spinner, card popup, UI interactions |
| **Bootstrap 5** | 5.3.2 | Responsive grid, buttons, forms, cards, navbar |
| **Font Awesome** | 4.7.0 | Icons (hamburger menu, GitHub icon, etc.) |
| **Google Fonts** | — | JetBrains Mono (body), Oswald & Lato (headings) |

### Django Add-on Packages

| Package | What It Does |
|---------|--------------|
| **django-crispy-forms** | Makes Django forms render with Bootstrap styling automatically |
| **crispy-bootstrap4** | Template pack for crispy-forms to use Bootstrap 4 layout |
| **django-widget-tweaks** | Lets you add CSS classes to form fields directly in HTML templates |
| **django-bootstrap-v5** | Bootstrap 5 integration utilities for Django |
| **whitenoise** | Serves static files (CSS, JS, images) efficiently in production |
| **python-dotenv** | Loads environment variables from `.env` file (like SECRET_KEY) |

### Encryption

| Technology | What It Does |
|-----------|--------------|
| **RC4 (Rivest Cipher 4)** | Custom-implemented stream cipher for encrypting messages before hiding |

### Database

| Technology | What It Does |
|-----------|--------------|
| **SQLite3** | Default lightweight database (stores image encoding records) |
| **PostgreSQL** (optional) | Production database option (commented out in settings) |

### Deployment

| Technology | What It Does |
|-----------|--------------|
| **Gunicorn** | Production-grade Python WSGI HTTP server |
| **Heroku** | Cloud platform (Procfile + runtime.txt configured for it) |

---

## 3. Project Folder Structure — Explained

```
video-steganography/               ← 📁 PROJECT ROOT
│
├── manage.py                      ← 🔧 Django's command-line tool (runs server, migrations, etc.)
├── requirements.txt               ← 📋 List of all Python packages needed
├── runtime.txt                    ← 📋 Tells Heroku which Python version to use (3.10.4)
├── Procfile                       ← 📋 Tells Heroku how to start the app (gunicorn)
├── .env                           ← 🔑 Secret environment variables (SECRET_KEY)
├── .gitignore                     ← 📋 Files/folders Git should ignore
├── db.sqlite3                     ← 🗄️ SQLite database file (auto-created)
│
├── camouflage/                    ← ⚙️ DJANGO PROJECT CONFIG (the "brain")
│   ├── __init__.py                ← Makes this folder a Python package
│   ├── settings.py                ← ⚙️ ALL project settings (apps, database, paths, etc.)
│   ├── urls.py                    ← 🗺️ ROOT URL router (sends URLs to correct app)
│   ├── wsgi.py                    ← 🚀 Entry point for production web server (WSGI)
│   └── asgi.py                    ← 🚀 Entry point for async web server (ASGI)
│
├── core/                          ← 📦 VIDEO STEGANOGRAPHY APP (main feature)
│   ├── __init__.py                ← Makes this folder a Python package
│   ├── admin.py                   ← Django admin panel registration
│   ├── apps.py                    ← App configuration (name = 'core')
│   ├── models.py                  ← Database models (currently empty/commented)
│   ├── forms.py                   ← 📝 EncodeForm & DecodeForm definitions
│   ├── views.py                   ← 🧠 ALL view functions + Video class (encode/decode logic)
│   ├── main.py                    ← (Empty file — logic moved into views.py)
│   ├── urls.py                    ← 🗺️ URL routes for this app (/encode/, /decode/, etc.)
│   ├── RC4/                       ← 🔐 ENCRYPTION SUB-MODULE
│   │   ├── __init__.py            ← Makes RC4 folder importable
│   │   └── rc4.py                 ← 🔐 RC4 cipher implementation (encrypt & decrypt)
│   └── templates/core/            ← 🎨 HTML TEMPLATES for video steganography
│       ├── index.html             ← 🏗️ BASE TEMPLATE (navbar, scripts, CSS — all pages extend this)
│       ├── home.html              ← 🏠 Landing page (hero section + explanations)
│       ├── encode.html            ← 📤 Video encoding form page
│       ├── decode.html            ← 📥 Video decoding form + result card popup
│       ├── sucess.html            ← ✅ Encoding success page (download + stats)
│       ├── about.html             ← ℹ️ About page
│       └── 404.html               ← ❌ Error page
│
├── imageSteg/                     ← 📦 IMAGE STEGANOGRAPHY APP (second feature)
│   ├── __init__.py                ← Makes this folder a Python package
│   ├── admin.py                   ← Django admin registration
│   ├── apps.py                    ← App configuration (name = 'imageSteg')
│   ├── models.py                  ← 🗄️ ImageEncoding model (stores image + message in DB)
│   ├── forms.py                   ← 📝 ImageEncodeForm & DecodeForm
│   ├── views.py                   ← 🧠 Image encode/decode logic (genData, modPix, encode, decode)
│   ├── urls.py                    ← 🗺️ URL routes (/image/steg/, /image/decode-image/, etc.)
│   └── templates/imageSteg/       ← 🎨 HTML TEMPLATES for image steganography
│       ├── imageEncode.html       ← 📤 Image encoding form
│       ├── imageDecode.html       ← 📥 Image decoding form + result
│       └── success.html           ← ✅ Image encoding success page
│
├── static/                        ← 🎨 STATIC FILES (CSS, JS, images)
│   ├── style.css                  ← 🎨 Main stylesheet (734 lines — all custom CSS)
│   ├── index.js                   ← ⚡ JavaScript (spinner, card popup, close button)
│   ├── bg.png                     ← 🖼️ Hero section background image
│   ├── steg.jpg                   ← 🖼️ Video steganography explanation image
│   ├── image.jpg                  ← 🖼️ Image steganography explanation image (Mona Lisa)
│   ├── about.avif                 ← 🖼️ About page image
│   ├── camouflage.png             ← 🖼️ Logo image
│   └── admin/                     ← Django admin panel static files (auto-generated)
│
└── media/                         ← 📂 USER-UPLOADED & PROCESSED FILES
    ├── videos/                    ← Uploaded original videos
    ├── images/                    ← Uploaded original images
    ├── encoded/                   ← Encoded video outputs
    │   └── image_encode/          ← Encoded image outputs (encoded.png)
    ├── decoded/                   ← Decoded outputs
    ├── icons/                     ← Application icons (favicon)
    ├── decode.txt                 ← Decoded video message text file
    └── image_decode.txt           ← Decoded image message text file
```

### What Each Top-Level Folder Does:

| Folder | Purpose | Analogy |
|--------|---------|---------|
| `camouflage/` | Project configuration | The **control room** — settings, URL routing |
| `core/` | Video steganography feature | The **main engine** — where video hiding/extracting happens |
| `imageSteg/` | Image steganography feature | The **second engine** — where image hiding/extracting happens |
| `static/` | CSS, JavaScript, images | The **wardrobe** — how the website looks |
| `media/` | User uploads & outputs | The **storage room** — files users upload and download |

---

## 4. How All Files Connect To Each Other

This is the most important section to understand. Here's how every file connects:

### The Complete Connection Chain:

```
USER types http://127.0.0.1:8000/encode/
         │
         ▼
    manage.py
    (starts Django server, loads settings from camouflage.settings)
         │
         ▼
    camouflage/settings.py
    (tells Django: "use camouflage.urls as root URL config")
    (tells Django: "core and imageSteg are installed apps")
    (tells Django: "static files are in /static, media in /media")
         │
         ▼
    camouflage/urls.py  ← ROOT URL ROUTER
    (sees /encode/ → matches path('', include("core.urls"))
     → forwards to core/urls.py)
         │
         ▼
    core/urls.py  ← APP URL ROUTER
    (sees /encode/ → matches path('encode/', views.encode))
    (calls the encode() function from core/views.py)
         │
         ▼
    core/views.py → encode() function
    (creates EncodeForm from core/forms.py)
    (if form submitted: calls Video.encode() which uses RC4/rc4.py)
    (renders template: core/templates/core/encode.html)
         │
         ▼
    core/templates/core/encode.html
    (extends core/templates/core/index.html ← base template)
    (index.html loads: static/style.css + static/index.js + Bootstrap CDN)
         │
         ▼
    USER sees the encode page in their browser
```

### Visual Connection Map:

```
┌─────────────────────────────────────────────────────────────────┐
│                     manage.py (Entry Point)                      │
│              "Start server with camouflage settings"             │
└────────────────────────────┬────────────────────────────────────┘
                             │ loads
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                  camouflage/settings.py                          │
│                                                                  │
│  INSTALLED_APPS = ['core', 'imageSteg', ...]                    │
│  ROOT_URLCONF = 'camouflage.urls'                               │
│  STATIC_URL = 'static/'                                         │
│  MEDIA_URL = '/media/'                                          │
│  DATABASES = { SQLite3 }                                        │
└────────────────────────────┬────────────────────────────────────┘
                             │ root URL config
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    camouflage/urls.py                            │
│                                                                  │
│  path('', include("core.urls"))        → Video steg pages       │
│  path('image/', include("imageSteg.urls"))  → Image steg pages  │
│  path('admin/', admin.site.urls)       → Django admin panel     │
│                                                                  │
│  + serves /static/ and /media/ files in debug mode              │
└───────────┬───────────────────────────────────┬─────────────────┘
            │                                   │
            ▼                                   ▼
┌───────────────────────┐           ┌───────────────────────────┐
│    core/urls.py       │           │    imageSteg/urls.py      │
│                       │           │                           │
│ /         → home()    │           │ /image/steg/      →       │
│ /encode/  → encode()  │           │     encode_request()      │
│ /decode/  → decode()  │           │ /image/decode-image/ →    │
│ /success/ → sucess()  │           │     decode_req()          │
│ /aboutus/ → about_us()│           │ /image/image-steg-success/│
└───────┬───────────────┘           │     → image_sucess()      │
        │                           └─────────┬─────────────────┘
        ▼                                     ▼
┌───────────────────────┐           ┌───────────────────────────┐
│    core/views.py      │           │    imageSteg/views.py     │
│                       │           │                           │
│ Uses:                 │           │ Uses:                     │
│ ├─ core/forms.py      │           │ ├─ imageSteg/forms.py     │
│ ├─ core/RC4/rc4.py    │           │ ├─ imageSteg/models.py    │
│ ├─ cv2 (OpenCV)       │           │ ├─ Pillow (PIL)           │
│ ├─ numpy              │           │ └─ os, time, re           │
│ ├─ moviepy            │           │                           │
│ └─ os, time, re, gc   │           │ Functions:                │
│                       │           │ ├─ genData()              │
│ Contains:             │           │ ├─ modPix()               │
│ ├─ Video class        │           │ ├─ encode_enc()           │
│ │  ├─ encode()        │           │ ├─ encode()               │
│ │  ├─ decode()        │           │ ├─ decode()               │
│ │  ├─ read_video()    │           │ ├─ encode_request()       │
│ │  └─ write_video()   │           │ └─ decode_req()           │
│ └─ View functions     │           └─────────┬─────────────────┘
└───────┬───────────────┘                     │
        │ renders                             │ renders
        ▼                                     ▼
┌───────────────────────┐           ┌───────────────────────────┐
│ core/templates/core/  │           │ imageSteg/templates/      │
│                       │           │   imageSteg/              │
│ index.html (BASE)◄────│───────────│──── (also extends this)   │
│   ├── home.html       │           │ imageEncode.html          │
│   ├── encode.html     │           │ imageDecode.html          │
│   ├── decode.html     │           │ success.html              │
│   ├── sucess.html     │           └───────────────────────────┘
│   ├── about.html      │
│   └── 404.html        │
└───────┬───────────────┘
        │ loads
        ▼
┌───────────────────────┐
│     static/           │
│ ├── style.css         │
│ ├── index.js          │
│ ├── bg.png            │
│ ├── camouflage.png    │
│ └── (other images)    │
└───────────────────────┘
```

### File Dependency Chain (Who Imports Whom):

```python
# manage.py imports:
#   → camouflage.settings (via DJANGO_SETTINGS_MODULE)

# camouflage/settings.py imports:
#   → dotenv (load_dotenv)           — to read .env file
#   → os                             — for file paths
#   → pathlib.Path                   — for BASE_DIR

# camouflage/urls.py imports:
#   → core.urls                      — includes video steg routes
#   → imageSteg.urls                 — includes image steg routes
#   → camouflage.settings            — for DEBUG, STATIC/MEDIA paths

# core/views.py imports:
#   → core.forms (EncodeForm, DecodeForm)    — form validation
#   → core.RC4.rc4 (RC4)                     — encryption/decryption
#   → cv2 (OpenCV)                           — video frame I/O
#   → numpy                                  — pixel array math
#   → moviepy.editor (VideoFileClip)         — audio extraction
#   → camouflage.settings                    — MEDIA_ROOT path
#   → PIL (Image)                            — imported but not used in video
#   → os, time, re, gc, json                 — utilities

# core/forms.py imports:
#   → django.forms                           — form field definitions
#   → FileExtensionValidator                 — restrict to .AVI files

# core/RC4/rc4.py imports:
#   → typing (List)                          — type hints only

# imageSteg/views.py imports:
#   → imageSteg.forms (ImageEncodeForm, DecodeForm)  — form validation
#   → imageSteg.models (ImageEncoding)                — via forms.py
#   → PIL (Image, UnidentifiedImageError)             — pixel manipulation
#   → hurry.filesize                                  — file size formatting
#   → os, time, re                                    — utilities

# imageSteg/forms.py imports:
#   → imageSteg.models (ImageEncoding)       — ModelForm needs the model
#   → FileExtensionValidator                 — restrict to png/jpg/jpeg

# imageSteg/models.py imports:
#   → django.db.models                       — database field types
#   → FileExtensionValidator                 — image file validation
```

---

## 5. Django Framework — How It Works Here

Django follows the **MVT (Model-View-Template)** design pattern. Here's how each part maps in this project:

### MVT Pattern Explained:

```
┌─────────────────────────────────────────────────────────────┐
│                    Django MVT Pattern                         │
│                                                              │
│   MODEL (Database)         VIEW (Logic)        TEMPLATE (UI) │
│   ┌──────────────┐     ┌──────────────┐     ┌─────────────┐ │
│   │ models.py    │     │ views.py     │     │ templates/  │ │
│   │              │◄────│              │────►│             │ │
│   │ What data    │     │ What happens │     │ What user   │ │
│   │ to store     │     │ when URL is  │     │ sees in     │ │
│   │              │     │ visited      │     │ browser     │ │
│   └──────────────┘     └──────┬───────┘     └─────────────┘ │
│                               │                              │
│                        ┌──────┴───────┐                      │
│                        │  urls.py     │                      │
│                        │ Which view   │                      │
│                        │ for which URL│                      │
│                        └──────────────┘                      │
└─────────────────────────────────────────────────────────────┘
```

### How Django Processes a Request (Step by Step):

```
Step 1: User visits http://127.0.0.1:8000/encode/

Step 2: Django checks camouflage/urls.py
        → path('', include("core.urls"))  matches!
        → Forwards remaining "encode/" to core/urls.py

Step 3: core/urls.py checks
        → path('encode/', views.encode)  matches!
        → Calls encode() function in core/views.py

Step 4: views.py encode() function runs:
        → Creates an EncodeForm
        → If GET request: just show the empty form
        → If POST request: validate form → process video → redirect

Step 5: views.py returns render(request, 'core/encode.html', context)
        → Django template engine loads encode.html
        → encode.html extends index.html (base template)
        → Variables from context (like {{ form }}) are inserted

Step 6: Browser receives the final HTML + CSS + JS
        → User sees the page
```

### The Settings File (camouflage/settings.py) — What Each Setting Does:

```python
# The most important settings explained:

SECRET_KEY = os.environ.get('SECRET_KEY')
# Security key for Django's cryptographic signing. Loaded from .env file.

DEBUG = True
# Shows detailed error pages. Set to False in production.

ALLOWED_HOSTS = ['*', '192.168.1.153:3000', 'camouflagenepa.xyz']
# Which domain names can access this app. '*' means all.

INSTALLED_APPS = [
    'core',              # Our video steganography app
    'imageSteg',         # Our image steganography app
    'django.contrib.admin',       # Django admin panel
    'django.contrib.auth',        # User authentication system
    'django.contrib.contenttypes',# Content type framework
    'django.contrib.sessions',    # Session management (stores encode results)
    'django.contrib.messages',    # Flash messages (success/error notifications)
    'django.contrib.staticfiles', # Serves CSS/JS/images
    'crispy_forms',               # Better form rendering
    'crispy_bootstrap4',          # Bootstrap 4 template for forms
    'widget_tweaks',              # Add CSS to form fields in templates
]

MIDDLEWARE = [
    'whitenoise.middleware.WhiteNoiseMiddleware',  # Serves static files in production
    'django.contrib.sessions.middleware.SessionMiddleware',  # Enables sessions
    'django.middleware.csrf.CsrfViewMiddleware',  # CSRF protection for forms
    # ... other security middleware
]

ROOT_URLCONF = 'camouflage.urls'  # Root URL configuration file

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',  # Using SQLite
        'NAME': BASE_DIR / 'db.sqlite3',          # Database file location
    }
}

STATIC_URL = 'static/'                          # URL prefix for static files
STATIC_ROOT = os.path.join(BASE_DIR, 'static')  # Where static files live on disk
MEDIA_URL = "/media/"                            # URL prefix for uploaded files
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')     # Where uploads are saved
```

---

## 6. Module-by-Module Breakdown

### Module 1: `camouflage/` — Project Configuration

This is NOT an app — it's the Django **project configuration**. Created by `django-admin startproject camouflage`.

| File | Purpose |
|------|---------|
| `settings.py` | Central configuration — database, apps, paths, middleware, security |
| `urls.py` | Root URL router — directs traffic to `core` or `imageSteg` apps |
| `wsgi.py` | WSGI server entry point — used by Gunicorn in production |
| `asgi.py` | ASGI server entry point — for async deployments |

**Key Role**: The `urls.py` here is the **first stop** for every URL. It says:
- URLs starting with nothing (`/`) → go to `core` app
- URLs starting with `/image/` → go to `imageSteg` app
- URLs starting with `/admin/` → go to Django's admin panel

---

### Module 2: `core/` — Video Steganography App

This is the **main app** handling video encoding and decoding.

#### `core/forms.py` — Form Definitions

```python
# Two forms defined here:

class EncodeForm:
    # Fields:
    video           → FileField (only .AVI allowed)
    secret_key      → CharField (max 10 characters)
    message         → CharField (max 80 characters, but validated to max 50)
    encoded_file_name → CharField (max 20, no extensions allowed)
    
    # Custom validation (clean method):
    # - Secret key must be < 10 characters
    # - Message must be ≤ 50 characters
    # - Filename must not end with .mp4, .avi, .png, etc.

class DecodeForm:
    # Fields:
    video           → FileField (only .AVI allowed)
    secret_key      → CharField (max 10 characters)
```

**Why forms?** Django forms automatically:
- Generate HTML form fields
- Validate input data (correct type, length, file extension)
- Show error messages to the user
- Protect against malicious input

---

#### `core/views.py` — View Functions & Video Class

This is the **biggest and most important file**. It contains:

**Simple View Functions:**
```python
def home(request):      → Renders home.html (landing page)
def about_us(request):  → Renders about.html
def sucess(request):    → Renders sucess.html (after encoding)
def error_404_view():   → Renders 404.html (page not found)
def error_500_view():   → Renders 404.html (server error)
```

**The Video Class** — Contains all steganography logic:
```python
class Video:
    rc4 = RC4()  # Creates one instance of the RC4 cipher

    read_video_frames(video_path)  → Reads video into frames + audio + fps
    encode(key, message, frames)   → Hides message in frames using LSB
    decode(key, frames)            → Extracts hidden message from frames
    write_video(request, frames, audio, output_path, fps)  → Saves frames as video
```

**The encode() view function** — Handles the encoding page:
```python
def encode(request):
    1. Create EncodeForm
    2. If form submitted and valid:
       a. Save uploaded video temporarily
       b. Read video frames using OpenCV
       c. Extract audio using MoviePy
       d. Encrypt message with RC4
       e. Hide encrypted message in frame pixels (LSB)
       f. Write new video with hidden data + original audio
       g. Store stats in session (completion time, file sizes)
       h. Redirect to success page
    3. If not submitted: show empty form
```

**The decode() view function** — Handles the decoding page:
```python
def decode(request):
    1. Create DecodeForm
    2. If form submitted and valid:
       a. Save uploaded encoded video temporarily
       b. Read all video frames
       c. Read metadata from Frame 0 (message length, start index)
       d. Extract encrypted bits from correct frames
       e. Decrypt using RC4 with provided key
       f. Validate result (check if printable ASCII)
       g. If valid: show decoded message
       h. If invalid: show error (wrong key or not encoded)
       i. Save decoded message to media/decode.txt
       j. Clean up temporary files
```

---

#### `core/RC4/rc4.py` — Encryption Module

Custom implementation of the **RC4 stream cipher**.

```python
class RC4:
    key_scheduler(key)          → Creates permutation array S[256] from key
    pseudo_random_gen(message)  → Generates pseudo-random keystream (generator)
    __call__(key, data, encrypt)→ XORs data with keystream
    encrypt(key, plaintext)     → Returns binary string of ciphertext
    decrypt(key, ciphertext)    → Returns plaintext string
```

**How encryption works here:**
```
"Hello" + key "abc"  →  encrypt()  →  "0110101011001010..."  (binary string)
"0110101011001010..." + key "abc"  →  decrypt()  →  "Hello"
```

---

#### `core/urls.py` — URL Routing

```python
urlpatterns = [
    path('',         views.home,     name='home'),      # /
    path('aboutus/', views.about_us, name='aboutus'),    # /aboutus/
    path('encode/',  views.encode,   name='encode'),     # /encode/
    path('decode/',  views.decode,   name='decode'),     # /decode/
    path('success/', views.sucess,   name='success'),    # /success/
]
```

The `name=` parameter lets templates reference URLs by name instead of hardcoding paths:
```html
<!-- In templates, instead of writing "/encode/", you write: -->
<a href="{% url 'encode' %}">Encode Video</a>
```

---

### Module 3: `imageSteg/` — Image Steganography App

Handles image encoding and decoding — simpler than video because:
- No audio to handle
- No encryption (RC4 is NOT used for images)
- Uses Pillow instead of OpenCV

#### `imageSteg/models.py` — Database Model

```python
class ImageEncoding(models.Model):
    image_file = models.FileField(upload_to='images/')  # Saves to media/images/
    message = models.CharField(max_length=80)            # The message to hide
```

**Why a model here but not in `core`?** The image steg app uses a `ModelForm` which automatically saves the uploaded image and message to the database. The video steg app doesn't save to the database.

#### `imageSteg/views.py` — Image Processing Logic

Key functions:

```python
genData(data)
    # Converts each character to 8-bit binary
    # "Hi" → ['01001000', '01101001']

modPix(pix, data)
    # Takes pixel data and message binary data
    # Modifies the LSB (last bit) of each pixel value
    # Uses groups of 3 pixels (9 values) per character
    # 8 values store the 8 bits of one character
    # 9th value signals: even LSB = more chars, odd LSB = last char

encode_enc(newimg, data)
    # Walks through image pixel by pixel
    # Calls modPix() to modify pixels
    # Writes modified pixels back to image

encode(request, files, data)
    # Opens original image
    # Creates a copy
    # Calls encode_enc() to hide message
    # Saves encoded image to media/encoded/image_encode/encoded.png

decode(image_file)
    # Opens encoded image
    # Reads pixels in groups of 3
    # Extracts LSBs to rebuild binary → characters
    # Stops when 9th value's LSB is odd (stop signal)
    # Returns the hidden message
```

#### `imageSteg/forms.py` — Form Definitions

```python
class ImageEncodeForm(forms.ModelForm):
    # Linked to ImageEncoding model
    # Fields: image_file (png/jpg/jpeg) + message (max 80 chars)

class DecodeForm(forms.Form):
    # Field: encoded_image (png/jpg/jpeg file)
```

---

### Module 4: `static/` — Frontend Assets

#### `static/style.css` (734 lines)

Organized into sections:
```css
/* Global styles */      → body font, box-sizing, link styles
/* Navbar styles */      → nav positioning, dropdown menus
/* Home page */          → hero section, animations, buttons
/* Animations */         → slide-in effects (messageAnimation, headingAnimation, etc.)
/* Encode/Decode page */ → form styling, input fields, buttons
/* Success page */       → output info, feature buttons
/* About page */         → layout, image positioning
/* Spinner overlay */    → loading spinner during encoding/decoding
/* Card popup */         → decoded message result card
/* Footer */             → footer layout
/* Responsive (mobile) */→ @media queries for screens < 1000px width
```

**Key CSS Features:**
- **Animations**: Slide-in effects when pages load (`@keyframes messageAnimation`, etc.)
- **Responsive Design**: Mobile layout via `@media (max-width: 1000px)`
- **Overlay Effects**: Semi-transparent black overlay for spinner and result card
- **Custom Fonts**: JetBrains Mono (body), Lato (headings), Oswald (footer)

#### `static/index.js` (28 lines)

```javascript
// Does 3 things:

// 1. Loading Spinner — Shows spinner overlay when form is submitted
document.getElementById('myform').addEventListener('submit', function() {
    document.getElementById('spinner-container').style.display = 'block';
});

// 2. Card Close Button — Hides the decoded message card when X is clicked
document.getElementById('closebtn').addEventListener('click', function() {
    element.style.display = 'none';
    document.getElementById('card-container').style.display = 'none';
});

// 3. Mobile Nav Toggle (not fully used — Bootstrap handles this)
```

---

## 7. The Core Algorithm — LSB Steganography

### What Is LSB?

Every pixel in a digital image/video has color values. For example, an RGB pixel:
- **Red** = 142 (binary: `10001110`)
- **Green** = 203 (binary: `11001011`)
- **Blue** = 87 (binary: `01010111`)

The **Least Significant Bit (LSB)** is the last bit (rightmost). Changing it only changes the color value by ±1, which is **invisible to the human eye**.

### Step-by-Step Example:

```
HIDING THE LETTER "A" (ASCII 65 = binary 01000001):

Original pixel values:    142,  203,  87,  255,  128,  64,  33,  190
In binary:             10001110  11001011  01010111  11111111  10000000  01000000  00100001  10111110
                              ^         ^         ^         ^         ^         ^         ^         ^
LSBs:                         0         1         1         1         0         0         1         0

Message bits to hide:         0         1         0         0         0         0         0         1

After XOR (change LSBs):
New binary:            10001110  11001011  01010110  11111110  10000000  01000000  00100000  10111111
New pixel values:         142,     203,     86,      254,     128,      64,      32,      191
                                          ↑         ↑                                    ↑         ↑
                              Changed: -1  Changed: -1              Changed: -1  Changed: +1

Visual difference: ZERO (you cannot see a color change of ±1)
```

### Video Steganography (How `Video.encode()` Works):

```
Step 1: Read all video frames → array of shape (num_frames, height, width, 3)
        Example: 900 frames × 1080 height × 1920 width × 3 colors = ~5.6 GB of data

Step 2: Encrypt message with RC4 → binary string
        "Hello" → "0101010010110001..." (40 bits for 5 chars × 8 bits)

Step 3: Pad message to fill complete frames
        Message is padded with spaces so it fills an exact number of frames

Step 4: Pick a RANDOM starting frame (between frame 1 and the end)
        This adds security — the message data isn't always in the same place

Step 5: XOR the encrypted binary data with the LSBs of target frames
        frames[random_index : random_index + N] are modified

Step 6: Store metadata in Frame 0:
        ┌────────────────────────────────┬──────────────────────────────┐
        │  Message Length (48 bits)      │  Random Start Index (48 bits)│
        │  Bits 0-47                     │  Bits 48-95                  │
        └────────────────────────────────┴──────────────────────────────┘
        Frame 0 is like a "table of contents" telling the decoder where to look

Step 7: Write modified frames back to video + original audio
```

### Video Decoding (How `Video.decode()` Works):

```
Step 1: Read all video frames

Step 2: Read Frame 0's LSBs:
        - First 48 bits → message length
        - Next 48 bits → random start index

Step 3: Go to frames[start_index] and read LSBs for 'length' number of bits

Step 4: Decrypt the extracted bits with RC4 using the provided key

Step 5: Return the plaintext message
```

### Image Steganography (How it's different):

```
- Uses groups of 3 PIXELS (9 color values) per character
- 8 values store the 8 bits of one character
- The 9th value acts as a STOP FLAG:
  - Even LSB (0) = "more characters coming"
  - Odd LSB (1) = "this was the last character"
- NO encryption (RC4 is not used)
- NO random positioning (data starts from pixel 0)
```

---

## 8. RC4 Encryption — How It Works

RC4 is a **stream cipher** — it generates a pseudo-random stream of bits (keystream) and XORs it with the message.

### The Three Steps:

```
┌─────────────────────────────────────────────────────────────┐
│  STEP 1: Key Scheduling Algorithm (KSA)                     │
│                                                              │
│  Input: Secret key ("mykey")                                │
│  Output: Shuffled array S[0..255]                           │
│                                                              │
│  Process:                                                    │
│  - Start with S = [0, 1, 2, ..., 255]                       │
│  - For each i from 0 to 255:                                │
│      j = (j + S[i] + key[i % key_length]) % 256            │
│      Swap S[i] and S[j]                                     │
│  - Result: key-dependent permutation of 0-255               │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2: Pseudo-Random Generation Algorithm (PRGA)          │
│                                                              │
│  Input: Shuffled S array                                    │
│  Output: Stream of pseudo-random bytes (keystream)          │
│                                                              │
│  Process (for each byte needed):                            │
│  - i = (i + 1) % 256                                       │
│  - j = (j + S[i]) % 256                                    │
│  - Swap S[i] and S[j]                                      │
│  - K = S[(S[i] + S[j]) % 256]   ← one keystream byte      │
│  - Yield K                                                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3: XOR with Message                                   │
│                                                              │
│  For ENCRYPTION:                                            │
│    plaintext_char XOR keystream_byte → 8-bit binary string  │
│    'H' (72) XOR 42 → 98 → '01100010'                       │
│                                                              │
│  For DECRYPTION:                                            │
│    binary_string → integer XOR keystream_byte → char        │
│    '01100010' → 98 XOR 42 → 72 → 'H'                       │
│                                                              │
│  KEY PROPERTY: XOR is reversible! A XOR B XOR B = A         │
│  Same key + same algorithm = same keystream = decryption    │
└─────────────────────────────────────────────────────────────┘
```

### Why RC4?

- **Symmetric**: Same key for encryption and decryption (simple for users)
- **Fast**: Stream cipher, processes one byte at a time
- **Adds security**: Even if someone knows LSB steganography is used, they can't read the message without the key

---

## 9. Complete Code Walkthrough

### File: `core/views.py` — Line by Line

#### The Video Class:

```python
class Video:
    rc4 = RC4()  # Class-level RC4 instance — shared by all methods
```

#### `read_video_frames()` — Reading a Video:

```python
@staticmethod
def read_video_frames(video_path):
    frames = []
    cap = cv2.VideoCapture(video_path)   # OpenCV opens the video file
    fps = cap.get(cv2.CAP_PROP_FPS)      # Get frames-per-second (e.g., 30)
    
    while True:
        ret, frame = cap.read()          # Read one frame (ret=True if successful)
        if not ret: break                # No more frames → stop
        frames.append(frame)             # Each frame is a numpy array (H × W × 3)
    
    cap.release()                        # Close OpenCV's video reader
    
    video_clip = VideoFileClip(video_path)  # MoviePy opens same video
    audio = video_clip.audio                # Extract audio track
    
    return frames, audio, fps            # Return everything
```

#### `encode()` — Hiding the Message:

```python
@staticmethod
def encode(key, message, frames):
    frames = np.array(frames)            # Convert list to NumPy array for fast math
    height, width, color_channel = frames[0].shape  # e.g., (1080, 1920, 3)
    
    # How many bits can we store? (all frames except frame 0)
    total_encodeable_amount = frames[1:,:,:,:].size
    total_encodeable_amount_single_frame = total_encodeable_amount // (len(frames)-1)
    
    # Check if message fits
    total_message_bits = len(message) * 8
    if total_encodeable_amount < total_message_bits:
        print("Error: Message too long")
        return frames
    
    # Pad message to fill exact number of frames
    extra_bits = total_message_bits % total_encodeable_amount_single_frame
    message += ' ' * ((total_encodeable_amount_single_frame - extra_bits) // 8)
    
    # Convert message length to 48-bit binary (for metadata)
    length_binary = bin(len(message))[2:].zfill(48)
    
    # ENCRYPT the message using RC4
    ciphertext = Video.rc4.encrypt(key=key, plaintext=message)
    
    # Pad ciphertext to align with color channels
    ciphertext += '0' * (len(ciphertext) % color_channel)
    
    # Reshape ciphertext into frame-shaped arrays
    ciphertext_frames = np.array(list(ciphertext)).astype(int)
    ciphertext_frames = ciphertext_frames.reshape((-1, height, width, color_channel))
    
    # Pick a RANDOM starting frame (for security)
    random_index = np.random.randint(1, len(frames) - ciphertext_frames.shape[0])
    
    # XOR ciphertext bits with frame LSBs (THE ACTUAL HIDING)
    binary_frame_lsb = frames[random_index: ciphertext_frames.shape[0]+random_index] % 2
    frames[random_index: ciphertext_frames.shape[0]+random_index] ^= \
        np.logical_xor(binary_frame_lsb, ciphertext_frames)
    
    # Store metadata in Frame 0 (message length + start index)
    random_index_binary = np.binary_repr(random_index, width=48)
    bin_array = np.array(list(length_binary + random_index_binary), dtype=np.uint8)
    binary_frame_lsb = frames[0] % 2
    bin_array = np.pad(bin_array, (0, frames[0].size - len(bin_array)), 'constant')
    bin_array = bin_array.reshape(frames[0].shape)
    frames[0] ^= np.logical_xor(binary_frame_lsb, bin_array)
    
    return frames
```

#### `decode()` — Extracting the Message:

```python
@staticmethod
def decode(key, frames):
    frames = np.array(frames)
    frame = frames[0]                    # Get metadata frame
    
    # Read all LSBs from frame 0
    unpacked_bits = np.unpackbits(frame)
    bin_array = ''.join(map(str, unpacked_bits.reshape((-1, 8))[:, -1]))
    
    # Extract metadata
    length_binary = bin_array[:48]       # First 48 bits = message length
    random_index_binary = bin_array[48:96]  # Next 48 bits = start frame
    
    length = int(length_binary, 2) * 8   # Convert to number of bits
    random_index = int(random_index_binary, 2)  # Convert to frame index
    
    # Calculate how many frames contain data
    total_encodeable_amount = frames[1,:,:,:].size
    total_frames = length // total_encodeable_amount
    
    # Read LSBs from the correct frames
    ciphertext = ""
    for frame in frames[random_index: random_index+total_frames]:
        unpacked_bits = np.unpackbits(frame)
        ciphertext += ''.join(map(str, unpacked_bits.reshape((-1, 8))[:, -1]))
    
    # DECRYPT using RC4
    plaintext = Video.rc4.decrypt(key=key, ciphertext=ciphertext)
    return plaintext
```

---

## 10. Request-Response Flow (What Happens When User Clicks)

### Flow 1: User Encodes a Video

```
USER ACTION                          WHAT HAPPENS IN CODE
───────────                          ────────────────────
1. Clicks "Encode Video"      →     Browser sends GET /encode/
   on home page                      → core/urls.py matches → views.encode()
                                     → EncodeForm created (empty)
                                     → encode.html rendered with empty form
                                     → User sees form in browser

2. Fills form + clicks        →     Browser sends POST /encode/ with:
   "Encode" button                    - video file (multipart upload)
                                     - secret_key, message, encoded_file_name
                                     index.js shows loading spinner

3. Server processing          →     views.encode():
                                     a) form.is_valid() checks:
                                        - Is file .AVI? Key < 10 chars?
                                        - Message ≤ 50 chars? No extension in filename?
                                     b) FileSystemStorage saves video to media/
                                     c) Video.read_video_frames() reads all frames
                                     d) Video.encode() hides encrypted message in LSBs
                                     e) Video.write_video() saves encoded video
                                     f) Session stores: completion time, file sizes, path
                                     g) Original upload deleted
                                     h) Redirects to /success/

4. Success page               →     Browser sends GET /success/
                                     → views.sucess() renders sucess.html
                                     → Template reads session data:
                                        {{ request.session.video_completion_time }}
                                        {{ request.session.encoded_video }}
                                     → User sees completion time + download button

5. User downloads              →     Clicks "Download Video"
                                     → Browser downloads /media/encoded/filename.avi
```

### Flow 2: User Decodes a Video

```
USER ACTION                          WHAT HAPPENS IN CODE
───────────                          ────────────────────
1. Navigates to Decode         →     GET /decode/ → empty DecodeForm rendered

2. Uploads encoded video       →     POST /decode/ with video + secret_key
   + enters key + clicks             index.js shows loading spinner
   "Decode"

3. Server processing           →     views.decode():
                                     a) Validates form (is .AVI? Has key?)
                                     b) Saves uploaded video temporarily
                                     c) Video.read_video_frames() reads frames
                                     d) Video.decode() extracts + decrypts message
                                     e) Checks if result is printable ASCII
                                     f) If YES: message displayed in card popup
                                        If NO: error shown (wrong key?)
                                     g) Saves result to media/decode.txt
                                     h) Cleans up temp file + frees memory (gc)

4. Result displayed            →     decode.html shows card popup:
                                     - Green "Decoded Successfully" or Red "Error"
                                     - The decoded message
                                     - "Export in Text" download button
                                     index.js enables card close button (X)

5. User exports (optional)    →      Clicks "Export in Text"
                                     → Downloads /media/decode.txt
```

---

## 11. Frontend — Templates & Styling

### Template Inheritance System:

```
index.html  ← BASE TEMPLATE (every page extends this)
├── Contains: <head> (CSS, fonts, Bootstrap, favicon)
├── Contains: <nav> (navbar with Home, Encode, Decode, About)
├── Contains: {% block content %} (empty — child templates fill this)
├── Contains: Flash messages display
├── Contains: <script> tags (Bootstrap JS, index.js)
│
├── home.html          {% extends 'core/index.html' %} → fills content block
├── encode.html        {% extends 'core/index.html' %} → fills content block
├── decode.html        {% extends 'core/index.html' %} → fills content block
├── sucess.html        {% extends 'core/index.html' %} → fills content block
├── about.html         {% extends 'core/index.html' %} → fills content block
├── imageEncode.html   {% extends 'core/index.html' %} → fills content block
├── imageDecode.html   {% extends 'core/index.html' %} → fills content block
└── image success.html {% extends 'core/index.html' %} → fills content block
```

### Key Django Template Tags Used:

```html
{% extends 'core/index.html' %}   → Inherit from base template
{% block content %}...{% endblock %} → Fill the content area
{% load static %}                  → Enable static file loading
{% static 'style.css' %}          → Get URL for a static file
{% url 'encode' %}                → Get URL for a named route
{% csrf_token %}                  → Security token for POST forms
{% render_field form.video %}     → Render a form field (widget_tweaks)
{{ form.video.errors }}           → Show validation errors for a field
{{ request.session.xxx }}         → Read data from the session
{% if messages %}...{% endif %}   → Conditionally show flash messages
{% for msg in messages %}         → Loop through messages
```

### CSS Animations:

```css
/* 4 animations on the home page: */

@keyframes headingAnimation {     /* Heading slides in from left */
    0%  { left: -500px; }
    100% { left: 0; }
}

@keyframes bodyAnimation {        /* Body text slides in from left */
    0%  { left: -700px; opacity: 0; }
    100% { left: 0; opacity: 1; }
}

@keyframes messageAnimation {     /* Main content slides in from left */
    0%  { left: -300px; }
    100% { left: 0; }
}

@keyframes opacAnimation {        /* Hero image fades in from right */
    0%  { opacity: 0; right: -500px; }
    100% { right: 0; opacity: 1; }
}
```

### Responsive Design:

```css
@media only screen and (max-width: 1000px) {
    /* Mobile layout: */
    /* - Top section becomes vertical (column) */
    /* - Font sizes increase for touch */
    /* - Forms become full width */
    /* - Bottom sections stack vertically */
    /* - About section becomes single column */
}
```

---

## 12. Database & Media Storage

### Database (SQLite3):

Django uses the database for:
1. **Django Admin**: User authentication, sessions
2. **ImageEncoding model**: Stores image file path + message for each encoding

```sql
-- The ImageEncoding table (auto-created by Django):
CREATE TABLE imageSteg_imageencoding (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    image_file  VARCHAR(100),    -- Path like "images/photo.png"
    message     VARCHAR(80)      -- The secret message
);
```

The video steganography does NOT use the database — it uses Django's **session framework** (stored in `django_session` table) to pass data between the encode view and the success page.

### Session Usage:

```python
# In views.py encode():
request.session['encoded_video'] = "/encoded/filename.avi"
request.session['original_size_video'] = 12.5   # MB
request.session['encoded_size_video'] = 13.2    # MB
request.session['video_completion_time'] = 4.5  # seconds

# In sucess.html:
{{ request.session.video_completion_time }}  → "4.5"
{{ request.session.encoded_video }}          → "/encoded/filename.avi"
```

### Media Directory Structure:

```
media/
├── videos/                    ← Uploaded original videos (temporary)
├── images/                    ← Uploaded original images (saved by ImageEncoding model)
├── encoded/                   ← Output: encoded videos
│   └── image_encode/          ← Output: encoded images (encoded.png)
├── decoded/                   ← (Created but not actively used)
├── icons/                     ← Favicon icon (lib.png)
├── decode.txt                 ← Decoded video message (text file export)
└── image_decode.txt           ← Decoded image message (text file export)
```

---

## 13. Deployment Configuration

### Files for Deployment:

```
Procfile       → Tells Heroku: "Run gunicorn camouflage.wsgi"
runtime.txt    → Tells Heroku: "Use Python 3.10.4"
.env           → Contains SECRET_KEY (not committed to Git)
wsgi.py        → WSGI entry point + WhiteNoise for static files
```

### How Production Differs:

| Aspect | Development | Production |
|--------|-------------|------------|
| Server | `manage.py runserver` | Gunicorn (via Procfile) |
| Static files | Django serves them | WhiteNoise serves them |
| Database | SQLite3 | PostgreSQL (commented option) |
| DEBUG | True | Should be False |
| SECRET_KEY | From .env | From environment variable |

---

## 14. Python Libraries & Why Each Is Needed

| Library | Import Statement | Used In | Purpose |
|---------|-----------------|---------|---------|
| **Django** | `from django.shortcuts import render` | views.py, forms.py, urls.py | The entire web framework |
| **OpenCV** | `import cv2` | core/views.py | Read video → frames, write frames → video |
| **NumPy** | `import numpy as np` | core/views.py | Fast array math: XOR, reshape, bit manipulation |
| **MoviePy** | `from moviepy.editor import VideoFileClip` | core/views.py | Extract audio from video, merge audio back |
| **Pillow** | `from PIL import Image` | imageSteg/views.py | Read/write image pixels for image steganography |
| **python-dotenv** | `from dotenv import load_dotenv` | settings.py | Load SECRET_KEY from .env file |
| **whitenoise** | `from whitenoise import WhiteNoise` | wsgi.py | Serve static files in production |
| **crispy-forms** | `{% load crispy_forms_tags %}` | templates | Auto-style Django forms with Bootstrap |
| **widget-tweaks** | `{% load widget_tweaks %}` | encode.html, decode.html | Add CSS classes to form fields in templates |
| **hurry.filesize** | `from hurry.filesize import size` | imageSteg/views.py | Format file sizes (imported but barely used) |
| **imageio-ffmpeg** | (used internally by MoviePy) | — | FFmpeg backend for video codec operations |
| **psycopg2-binary** | (not actively used) | — | PostgreSQL driver (for production database) |
| **beautifulsoup4** | (not actively used in views) | — | HTML parsing utility |
| **requests** | (not actively used in views) | — | HTTP requests library |

---

## 15. Common Questions For Presentation

### Q: What is the main concept behind this project?
**A:** Steganography — hiding information inside media files. We use the LSB (Least Significant Bit) technique to modify pixel values by ±1, which is invisible to the human eye but can store binary data.

### Q: What makes this different from encryption?
**A:** Encryption makes data unreadable (scrambled text). Steganography makes data **invisible** — the video/image looks completely normal. This project combines BOTH: RC4 encryption + LSB steganography for double security.

### Q: Why AVI format only for videos?
**A:** AVI is a lossless format — it preserves every pixel value exactly. Lossy formats like MP4 compress pixels, which would destroy the hidden data in the LSBs. The project uses FFV1 codec (lossless) when writing the encoded video.

### Q: How much data can be hidden?
**A:** Current limits: Video messages ≤ 50 characters, Image messages ≤ 80 characters. Theoretically, a single 1920×1080 frame can store ~760 KB of data in its LSBs, so much more could be hidden.

### Q: What happens if someone uses the wrong key?
**A:** The RC4 decryption with the wrong key produces garbled binary. The code checks if the result contains only printable ASCII characters (`re.match("^[ -~\\s]+$", decoded_message)`). If not, it shows an error.

### Q: What are the two main Django apps?
**A:** `core` (video steganography with RC4 encryption) and `imageSteg` (image steganography without encryption). They share the same base template but have separate views, forms, URLs, and templates.

### Q: How does the frontend communicate with the backend?
**A:** Through HTML forms with `POST` method. When the user submits a form, the browser sends a multipart request (for file upload). Django validates the data using form classes, processes it, and returns an HTML response.

### Q: What is Django's session used for here?
**A:** To pass data between the encode view and the success page. After encoding, stats like completion time and file sizes are stored in `request.session`, which the success page template reads and displays.

### Q: Is this project production-ready?
**A:** It has production deployment files (Procfile, Gunicorn, WhiteNoise), but the image steganography module has a hardcoded Windows path that needs fixing, and the SECRET_KEY should be properly configured.

---

## Summary Table: Every File and Its Role

| # | File Path | Role | Connected To |
|---|-----------|------|-------------|
| 1 | `manage.py` | Django CLI entry point | `camouflage/settings.py` |
| 2 | `camouflage/settings.py` | Project configuration | All apps, database, static/media paths |
| 3 | `camouflage/urls.py` | Root URL router | `core/urls.py`, `imageSteg/urls.py` |
| 4 | `camouflage/wsgi.py` | Production server entry | `settings.py`, WhiteNoise |
| 5 | `core/urls.py` | Video steg URL routes | `core/views.py` |
| 6 | `core/views.py` | Video encode/decode logic | `forms.py`, `RC4/rc4.py`, templates |
| 7 | `core/forms.py` | Form validation | `views.py` |
| 8 | `core/RC4/rc4.py` | Encryption/decryption | `views.py` (Video class) |
| 9 | `core/templates/core/index.html` | Base template (navbar+scripts) | All other templates |
| 10 | `core/templates/core/home.html` | Home/landing page | `index.html` |
| 11 | `core/templates/core/encode.html` | Video encode form | `index.html`, `views.py` |
| 12 | `core/templates/core/decode.html` | Video decode form+result | `index.html`, `views.py` |
| 13 | `core/templates/core/sucess.html` | Encode success page | `index.html`, session data |
| 14 | `core/templates/core/about.html` | About page | `index.html` |
| 15 | `imageSteg/urls.py` | Image steg URL routes | `imageSteg/views.py` |
| 16 | `imageSteg/views.py` | Image encode/decode logic | `forms.py`, `models.py`, templates |
| 17 | `imageSteg/forms.py` | Image form validation | `views.py`, `models.py` |
| 18 | `imageSteg/models.py` | Database model | `forms.py`, database |
| 19 | `imageSteg/templates/...` | Image steg pages | `core/index.html`, `views.py` |
| 20 | `static/style.css` | All CSS styling | Every template (via `index.html`) |
| 21 | `static/index.js` | Spinner + card popup | Every template (via `index.html`) |
| 22 | `requirements.txt` | Python dependencies | pip install |
| 23 | `.env` | Secret key | `settings.py` (via dotenv) |
| 24 | `Procfile` | Heroku deployment | Gunicorn server |
| 25 | `runtime.txt` | Python version | Heroku deployment |

---

*This guide covers every aspect of the Camouflage Video & Image Steganography project. Use it to understand, explain, and present the project confidently to any audience.*
