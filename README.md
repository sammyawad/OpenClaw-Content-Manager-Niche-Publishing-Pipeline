# OpenClaw Content Manager: Cinematic Video Pipeline

## Overview
The OpenClaw Content Manager is an autonomous, local-first orchestration system designed to operate a "Cash Cow" YouTube Network. Built on the OpenClaw framework, this system manages the entire lifecycle of high-retention video content: trend research, scriptwriting, voice synthesis, video frame generation, and final assembly.

**The Hardware Advantage:** Standard automated video pipelines fail due to the massive API burn rate of cloud video generation (e.g., Runway Gen-3). This architecture completely bypasses those costs by executing VRAM-heavy video diffusion models locally on enthusiast-grade hardware (NVIDIA RTX 5090 32GB / Ryzen 7 9800X3D). 

## Core Architecture

The system utilizes a central OpenClaw "Manager" agent to orchestrate the following pipeline:

### 1. The Trend Spotter (Local Inference)
* **Objective:** Identify high-velocity topics with strong retention potential.
* **Mechanism:** OpenClaw agents scrape YouTube API data and trending metrics.
* **Compute:** Local execution via `vLLM` or `Ollama` (Llama-3 8B) for zero-cost, high-volume data parsing.

### 2. The Retention Scriptwriter (Cloud API)
* **Objective:** Generate highly engaging, retention-optimized video scripts.
* **Mechanism:** The locally-validated topic is routed to the Anthropic API (Claude 3.5 Sonnet) to draft a script with precise scene breakdowns and a strong 5-second hook.

### 3. Audio & Cinematic Generation (Local GPU / MCP)
* **Voice Engine:** Local execution of XTTSv2 or F5-TTS to generate high-fidelity voiceovers.
* **Video Engine (The Heavy Lifter):** Utilizing the OpenClaw Model Context Protocol (MCP), the Manager commands a local instance of **CogVideoX-5B** or **Stable Video Diffusion**. The RTX 5090 generates 4-second B-roll clips for every scene specified in the script.

### 4. Automated Assembly (Python)
* **Objective:** Stitch raw assets into a cohesive, publish-ready MP4.
* **Mechanism:** A custom Python worker utilizes `MoviePy` to dynamically calculate the audio length and splice the generated 4K B-roll clips to match the pacing of the voiceover, injecting dynamic subtitles.

### 5. Distribution & Security (HITL)
* **Objective:** Secure YouTube uploads avoiding "Reused Content" or bot bans.
* **Mechanism:**
  * **Session Mirroring:** Playwright headless browser automation injecting preserved session cookies.
  * **Human-in-the-Loop (HITL):** A Telegram bot integration. The OpenClaw Manager pauses the pipeline, sends the finalized MP4 to the operator's device, and waits for a strict `YES` command before uploading.

## Tech Stack
* **Orchestration:** OpenClaw (Daemon & Memory Management)
* **Inference:** Ollama (Llama-3), CogVideoX / SVD, F5-TTS
* **Assembly & Automation:** Python (`MoviePy`), Playwright
