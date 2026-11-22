# BloodBorne PC

Native x86-64 PC port of *Bloodborne* (CUSA03173, v1.09) built on a Wine + DXVK style architecture: the original PlayStation 4 executable runs directly on your CPU while PS4 system libraries and graphics calls are translated to native Vulkan.

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/340c92c5-4f2f-46fe-801f-f7efc83ed2d3" />

<img width="1920" height="1280" alt="image" src="https://github.com/user-attachments/assets/501f7958-104a-4c4c-a8ce-96cb9b21530c" />

<img width="640" height="360" alt="image" src="https://github.com/user-attachments/assets/9c2c0eb1-e985-425c-8dd1-0ef1126535dc" />

## Native Execution & Vulkan Translation

Unlike traditional console emulators that interpret or recompile every CPU instruction, this port converts the original `eboot.bin` offline into a flat x86-64 memory image and links PS4 runtime shims (`libc`, `libSceFios2`, audio, and gamepad input) natively.

- **Full-Speed CPU Path**: Zero instruction-level CPU emulation overhead; Yharnam and the Hunter's Dream run directly on host x86-64 hardware.
- **Temporal Upscaling (FSR 3.1 / 4 / 4.1.1)**: Because the PS4 title lacks a native velocity buffer, the renderer computes camera motion vectors from depth and scene matrices plus object motion from prior vertex positions, applying Halton sub-pixel jitter and drawing the HUD natively at output resolution (720p, 1080p, 1440p, or 2160p).
- **Unlocked Frame Rates**: Community frame-time patches in `files/patches/Bloodborne.xml` decouple simulation ticks from 30 FPS, enabling 60, 90, or 150 FPS modes depending on GPU headroom.

<img width="739" height="415" alt="image" src="https://github.com/user-attachments/assets/6c91aa9f-3926-47ae-9de2-43dc6e03df16" />
<img width="625" height="320" alt="image" src="https://github.com/user-attachments/assets/5f3039fc-0154-4ee7-91de-32361446216e" />

## Memory Models & GPU Configuration

| Parameter | Default (0.2 Model) | Experimental PC Model (`BB_GUEST_IN_PLACE=1`) |
| --- | --- | --- |
| Guest Data Location | VRAM copies with write tracking | System RAM with hot textures cached in VRAM |
| Unused Texture Eviction | Freed after 20 seconds | Freed after 20 seconds |
| Unused Buffer Eviction | Freed after 60 seconds | Freed after 60 seconds |
| Recommended Hardware | NVIDIA, AMD, Steam Deck | AMD RDNA / Steam Deck (experimental) |

<img width="739" height="415" alt="image" src="https://github.com/user-attachments/assets/407a580c-6f65-40f6-9f40-fbbad1d82202" />

<img width="735" height="417" alt="image" src="https://github.com/user-attachments/assets/03ad8726-c216-4750-a9a9-fe42ac470dc0" />

## Visual Fixes in Build 1.9.8.6

> No copyrighted game assets are distributed in this repository. Provide your own legal CUSA03173 v1.09 dump before launching.

1. **Smeared Frame Elimination**: Every submitted GPU command buffer is now copied prior to execution (`BB_COPY_GPU_BUFFERS=0` disables this), stopping overwritten-buffer smearing and `Unimplemented PM4 type 0` log errors.
2. **Cold Shader Cache Recovery**: Fixed a pipeline-loss race on empty shader caches that previously caused motion blur to freeze on the first camera pan.
3. **Artifact & Cutscene Cleanup**: Resolved rainbow tile corruption on struck enemies, removed the bright top-edge line in cutscenes, and aligned Vulkan shader NaN/infinity clamping with PS4 GPU behavior.

<img width="739" height="415" alt="image" src="https://github.com/user-attachments/assets/e8690ccb-5ff0-40af-8c78-94d796779031" />

## Quick Launch Steps

1. Place your extracted CUSA03173 v1.09 game directory in a writable local path.
2. Review upscaling presets in `files/vulkan/fsr4_preset.toml` and memory flags in `files/memory/guest_model.ini`.
3. Enable `UnlockedFPS` inside `files/patches/Bloodborne.xml` if targeting 60+ FPS.
4. Launch the executable and verify Vulkan device selection in the startup log.

<img width="739" height="415" alt="image" src="https://github.com/user-attachments/assets/d5bda8cd-2284-4a9a-b3a2-0fb3695e9e6e" />
