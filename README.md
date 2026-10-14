<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6503ff,100:ff0064&height=150&section=header" width="100%" />

<img src="logo.png" alt="HOLO✦PAD logo" width="150" />

# HOLO✦PAD

**Pull your images out of the screen and into space.**
A browser-native spatial interface where you build in 3D with nothing but your hands.

<br/>

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Three.js](https://img.shields.io/badge/Three.js-000000?style=for-the-badge&logo=threedotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-0097A7?style=for-the-badge&logo=google&logoColor=white)

[![Live Demo](https://img.shields.io/badge/▶%20Live%20Demo-holopad.vercel.app-ff0064?style=flat-square)](https://holopad.vercel.app)
[![Status](https://img.shields.io/badge/Status-Prototype%20v1.0-6503ff?style=flat-square)](#-about)
[![GitDiagram](https://img.shields.io/badge/Explore%20on-GitDiagram-0d0221?style=flat-square)](https://gitdiagram.com/shiv0xparanoid/HOLOPAD)

[About](#-about) · [Features](#-features) · [Workflow](#-workflow) · [Tech Stack](#-tech-stack) · [Structure](#-project-structure) · [Run Locally](#-run-locally) · [Vision](#-vision)

</div>

<br/>

## ✦ About

HOLO✦PAD is an early-stage **spatial computing interface** exploring real-time graphics, mathematical models and human-computer interaction. It bridges the gap between flat 2D imagery and interactive 3D space.

Upload an image and it becomes a textured 3D hologram. Step into the **Creative Space** and build voxel by voxel, in mid-air, using hand gestures through your webcam.

<br/>

## ✦ Features

| | Feature | What it does |
|---|---|---|
| 🖼️ | **Image → Hologram** | Upload any image, and AI segmentation (SAM) plus mathematical extrusion produce a textured 3D model (GLB) |
| 🧱 | **Creative Space** | A fully functional voxel editor for building structures pixel by pixel in 3D |
| ✋ | **Hand Tracking** | Touchless pinch, draw and manipulate using MediaPipe Hands |
| 🌐 | **AR / Passthrough** | Overlay 3D content on the real world through the device webcam |
| 📸 | **Instant Capture** | Built-in screenshot tool to capture and share AR creations |
| 🔊 | **Spatial Audio** | Soundscapes powered by Howler.js |
| 🤖 | **MEG, AI Companion** | A holographic avatar with RAG memory, lip-sync, mood animations and mic input |

<br/>

## ✦ Workflow

> 🔍 **Interactive version:** open this repo on **[GitDiagram](https://gitdiagram.com/shiv0xparanoid/HOLOPAD)** for the auto-generated, clickable architecture view. (Replace `github.com` with `gitdiagram.com` in any repo URL.) The diagrams below are the curated versions.

### 1 · System architecture

```mermaid
flowchart TB
    subgraph WEB["🖥️  web · React + Vite"]
        direction TB
        R3F["Three.js · React-Three-Fiber<br/>WebGL scene"]
        CS["Creative Space<br/>voxel editor"]
        HOLO["Hologram Viewer<br/>GLB models"]
        HT["Hand Tracking<br/>MediaPipe Hands"]
        AR["AR Passthrough<br/>webcam"]
        AUD["Spatial Audio<br/>Howler.js"]
        CAP["Instant Capture"]
        R3F --- CS & HOLO
        HT --> CS
        AR --> R3F
        AUD --> R3F
        R3F --> CAP
    end

    subgraph SRV["⚙️  server · Node + Express"]
        direction TB
        EXP["Express API"]
        PY["Python worker<br/>SAM segmentation"]
        EXP --> PY
    end

    CAM(["📷 Webcam"]) --> HT
    CAM --> AR
    USER(["🧑 User"]) -->|"upload image"| WEB
    WEB -->|"image"| SRV
    SRV -->|"textured GLB"| WEB

    classDef web fill:#1a0540,stroke:#6503ff,color:#fff
    classDef srv fill:#3d0020,stroke:#ff0064,color:#fff
    classDef io fill:#0d0221,stroke:#888,color:#fff
    class R3F,CS,HOLO,HT,AR,AUD,CAP web
    class EXP,PY srv
    class CAM,USER io
```

### 2 · Image → Hologram pipeline

```mermaid
flowchart LR
    A["🖼️ Upload<br/>image"] --> B["Express API"]
    B --> C["SAM<br/>AI segmentation"]
    C --> D["Subject mask"]
    D --> E["Mathematical<br/>extrusion"]
    E --> F["Texture mapping"]
    F --> G["📦 GLB model"]
    G --> H["Three.js<br/>hologram view"]

    classDef a fill:#1a0540,stroke:#6503ff,color:#fff
    classDef b fill:#3d0020,stroke:#ff0064,color:#fff
    class A,H,G a
    class B,C,D,E,F b
```

### 3 · Gesture → creation loop

```mermaid
sequenceDiagram
    autonumber
    actor U as User
    participant C as Webcam
    participant M as MediaPipe Hands
    participant S as Creative Space (Three.js)
    participant V as Viewport

    loop every frame
        C->>M: video frame
        M->>S: hand landmarks
        S->>S: detect pinch / move / draw
        S->>S: place or move voxel
        S->>V: render scene (+ AR overlay)
    end
    U->>V: capture
    V-->>U: screenshot
```

<br/>

## ✦ Tech Stack

| Layer | Tools |
|---|---|
| **Frontend** | React · Vite · Three.js · React-Three-Fiber |
| **Computer Vision** | MediaPipe Hands (Google) |
| **Backend / AI** | Node.js · Express · Python (SAM integration) |
| **Audio** | Howler.js |
| **Hosting** | Vercel |

<br/>

## ✦ Project Structure

```
HOLOPAD/
├── web/               # React + Vite frontend · 3D scene, voxel editor, hand tracking
├── server/            # Node + Express API · image to hologram pipeline
├── PITCH_GUIDE.md     # pitch notes
├── Holopad.pdf        # project deck
├── logo.png
└── package.json
```

<br/>

## ✦ Run Locally

> **Needs:** Node 18+, Python 3 (for the SAM pipeline), a webcam for hand tracking.

```bash
git clone https://github.com/shiv0xparanoid/HOLOPAD.git
cd HOLOPAD

# frontend
cd web
npm install
npm run dev

# backend (new terminal)
cd server
npm install
npm start
```

Or skip setup and try it live at **[holopad.vercel.app](https://holopad.vercel.app)**.

> 💡 Hand tracking and AR mode need camera permission. Use Chrome or Edge on a device with a decent webcam for the best results.

<br/>

## ✦ Vision

The goal is to move interaction away from flat screens and into physical space, where **depth, motion and intent** are captured directly.

<br/>

<div align="center">

Built by **[Shivang](https://github.com/shiv0xparanoid)**, Founder of HOLO✦PAD

<sub>If this made you want to reach into your screen, drop a ⭐</sub>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:ff0064,100:6503ff&height=120&section=footer" width="100%" />

</div>
