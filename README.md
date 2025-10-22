# Raspberry Pi MagicMirror with GPT Voice Assistant

Smart mirror built from cedar wood and powered by a Raspberry Pi 3 B+ running MagicMirror² with an integrated GPT-based voice assistant.

---

## Table of Contents
1. [Overview](#overview)
2. [Frame Construction](#frame-construction)
3. [Electronics](#electronics)
4. [Software Setup](#software-setup)
5. [GPT Integration Details](#gpt-integration-details)
6. [Photos](#photos)

---

## Overview
A freestanding smart mirror displaying time, weather, and news while responding to voice commands through a custom GPT-powered assistant.  
The project combines a hand-built cedar frame with MagicMirror² (v2.33.0) software and a custom module that listens for the wake word **“mirror.”**

**Purpose**
- Functional and aesthetic smart display  
- Real-time weather, clock, and news  
- Conversational voice assistant with visual and spoken output  

---

## Frame Construction

| Component | Detail |
| ---------- | ------- |
| **Material** | Cedar wood |
| **Outer dimensions** | 68 in × 26 in |
| **Corners** | 45° miter joints (cut with miter saw) |
| **Groove for plexiglass** | 1/8 in deep on all four pieces (cut on table saw) |
| **Mirror layer** | Clear plexiglass with one-way mirror film applied |
| **Assembly** | Plexiglass captured by all four frame sides; held securely once corners are joined |
| **Finish** | Sanded and stained with red-oak finish |
| **Back side** | Open design with removable wood brace holding monitor and Raspberry Pi in place |
| **Maintenance** | Brace unscrews for easy monitor removal or wiring adjustments |

---

## Electronics

| Component | Use |
| ---------- | --- |
| **Acer 24″ display** | Main screen behind one-way mirror |
| **Raspberry Pi 3 B+** | Runs MagicMirror² and custom GPT assistant |
| **USB microphone** | Room-voice pickup (wake word + questions) |
| **Speaker** | Plays synthesized replies |
| **PIR sensor** | Detects motion to wake display |
| **Connections** | HDMI from Pi to display; AC to wall outlet |
| **Mounting** | Display and Pi attached to rear support brace; wiring routed cleanly along back |

---

## Software Setup

- **MagicMirror² Version:** 2.33.0  
- **Custom Module:** `MMM-GPTAssistant` located in `modules/MMM-GPTAssistant/`
- **Wake Word:** `mirror`

**Behavior**
- Idle state displays **“Say ‘mirror’.”**
- On wake, a **thin blue bar** appears at the top and status changes to **“Listening…”** (auto-times out after ~10s).
- Transcribed input is sent to **OpenAI GPT-4o-mini** and the assistant’s **text reply** shows centered on-screen in a clean, outline-only bubble; reply is also **spoken aloud**.
- Returns to **idle** after completion or timeout.

**Other Modules**
- Weather, Clock, Newsfeed remain active with standard MagicMirror layout.  
- The assistant occupies the lower/center region without cluttering other widgets.

---

## GPT Integration Details

**Architecture (3 parts)**
1. **Frontend (MagicMirror UI)**
   - Minimal status line (“Say ‘mirror’” → “Listening…”).
   - Slim blue listening bar at the top when active.
   - Centered outline-only caption area for the assistant’s reply.

2. **Node Helper (`node_helper.js`)**
   - Spawns the Python assistant process.
   - Forwards assistant events to the UI:
     - `::WAKE` → show “Listening…” + blue bar  
     - `::RESP <text>` → display final reply text  
     - `::READY` → return to idle

3. **Python Assistant (`assistant.py`)**
   - **Audio:** `sounddevice` for mic capture (USB lavalier mic).
   - **Wake & STT:** **Vosk** for local wake-word detection and offline transcription.
   - **LLM:** **OpenAI GPT-4o-mini** for response generation (text).
   - **TTS:** Fetches MP3 via OpenAI’s TTS endpoint; playback with `ffmpeg`/`ffplay`.
   - **Environment:** Uses `OPENAI_API_KEY` from environment (keeps secrets out of code).
   - **Runtime:** Optional Python `venv` for dependencies; **PM2** keeps MagicMirror and the assistant running on boot.

---

## Photos


