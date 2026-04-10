# OpenClaw Content Manager: Niche Publishing Pipeline

## Overview
The OpenClaw Content Manager is an autonomous, local-first agent orchestration system designed to manage a complete digital publishing pipeline. Built on the [OpenClaw](https://github.com/your-link-here) framework, this system coordinates multiple AI models to handle market research, long-form content generation, and automated distribution.

By leveraging enthusiast-grade local hardware (RTX 5090 / Ryzen 7 9800X3D), this architecture drastically reduces cloud API burn rates by routing high-volume data parsing to local inference models (Llama-3), while reserving premium cloud APIs (Claude 3.5 Sonnet) strictly for high-fidelity creative output.

## Core Architecture

This system utilizes a "Manager" agent to delegate tasks across four distinct operational phases:

### 1. Market Intelligence (Local Inference)
* **Objective:** Identify high-intent, low-competition topics based on public search volume.
* **Mechanism:** OpenClaw agents utilize official Data APIs (Serper/PyTrends, Reddit API) to scrape engagement metrics.
* **Compute:** Heavy data parsing and summary generation are executed locally via `vLLM`/`Ollama` (Llama-3 8B/70B) to maintain a zero-cost research loop.

### 2. Content Generation (Cloud API)
* **Objective:** Architect high-quality, long-form manuscripts (15,000+ words).
* **Mechanism:** The Manager agent passes the locally-generated, validated outlines to the Claude 3.5 Sonnet API for creative drafting, ensuring professional-grade prose.

### 3. Visual Assets (Local API / MCP)
* **Objective:** Generate high-end, artistic book covers.
* **Mechanism:** Utilizing the OpenClaw Model Context Protocol (MCP), the system interfaces directly with a locally hosted instance of Stable Diffusion 3 Medium or Flux.1, leveraging the RTX 5090's 32GB VRAM for rapid, cost-free asset generation.

### 4. Automated Distribution & Security
* **Objective:** Secure, automated uploads to Amazon KDP and IngramSpark without triggering bot-detection bans.
* **Mechanism:** * **Session Mirroring:** Utilizes Python and Playwright for browser automation, injecting saved session cookies and local storage to mirror an established, authenticated human session.
  * **Human-in-the-Loop (HITL):** A mandatory approval gate via Telegram API. The OpenClaw Manager pauses the pipeline and sends a summary/preview to the operator's device. No content is published or uploaded without explicit `YES` confirmation.

## Why OpenClaw?
Standard LLM wrappers are stateless. This pipeline requires a persistent daemon capable of interacting with the local file system and executing secure, sandboxed commands. 
* **State Management:** The OpenClaw Gateway natively handles session routing and memory, allowing the Manager agent to coordinate multi-day asynchronous tasks.
* **Data Sovereignty:** API keys, session cookies, and proprietary prompt matrices remain completely air-gapped on the host machine.
* **Extensibility:** Native integration for Telegram (HITL) and seamless tool injection via the ClawHub plugin ecosystem.

## Tech Stack
* **Orchestration:** OpenClaw 
* **Local Inference:** Ollama / vLLM (Llama-3)
* **Cloud Inference:** Anthropic API (Claude 3.5 Sonnet)
* **Automation:** Python, Playwright
* **Hardware Profile:** Optimized for NVIDIA RTX 50-series (local LLM/Image generation) and high-thread-count CPUs (Ryzen 9000 series).

## Current Phase: Slice 1 - The Research Loop
* [ ] Integrate PyTrends/Serper API via OpenClaw skill.
* [ ] Configure local Llama-3 parsing logic.
* [ ] Implement Telegram HITL bot for topic approval.
