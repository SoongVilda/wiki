---
title: Chromium-Based Browsers HW Acceleration
description: Configure hardware acceleration for video decode/encode in Chromium-based browsers on CachyOS. Includes AMD GPU setup and a template for other GPUs/browsers.
---

# Chromium-Based Browsers HW Acceleration

This guide outlines enabling hardware acceleration in Chromium-based browsers on CachyOS. This offloads video/graphics tasks to your GPU, improving performance.

## Prerequisites

* **Chromium-based Browser:** (e.g., Chrome, Brave, Ungoogled Chromium, Edge)
* **GPU Drivers/APIs:** Up-to-date Mesa (AMD/Intel) or NVIDIA drivers, with Vulkan/VA-API/VDPAU configured.
* **`amdgpu_top` (for AMD users):** Install `amdgpu_top` from the repository through package manager if you wish to monitor AMD GPU activity from the terminal.

## Contribution

This guide is extensible. If you have a working hardware acceleration setup for a specific GPU and Chromium-based browser, contribute by adding a new section under "GPU & Browser Configurations." Include:

* **Browser Name**
* **GPU Model**
* **Flags:** `~/.config/[browser]-flags.conf` content.
* **File Path:** Full path to the flags file.
* **Notes (Optional):** Key drivers, packages, or setup specifics.

## Setup Steps

1.  **Identify Flags File:** Locate your browser's flags file path in "GPU & Browser Configurations."
2.  **Edit Flags File:** Open/create the file using `nano` (or your preferred text editor like `micro`, `vim`).
    ```bash
    nano [PATH_TO_YOUR_BROWSER_FLAGS_FILE]
    # Example: nano ~/.config/chrome-flags.conf
    ```
3.  **Add Flags:** Paste the relevant GPU/browser flags into the file.
4.  **Save & Close.**
5.  **Restart Browser:** Close all browser instances and relaunch.
6.  **Verify:** Navigate to `chrome://gpu` (or `brave://gpu`, `edge://gpu`, etc.). Confirm "Hardware accelerated" status under "Video Acceleration Information" and "Graphics Feature Status."

## Verification Tips

To definitively check if hardware acceleration is active during video playback, use these methods:

### 1. Check AMD GPU Utilization (`amdgpu_top`)

If you have an AMD GPU and `amdgpu_top` installed, open a terminal and run it:

```bash
amdgpu_top
````

While a video is playing in your browser (e.g., YouTube), observe the `media` section in `amdgpu_top`. You should see some utilization here, indicating your GPU's media engine is active. If it remains at 0% during video playback, hardware acceleration might not be fully engaged for decoding.

### 2. Check Browser Developer Tools (Video Decoder)

This method provides direct confirmation from the browser itself:

1. Open your Chromium-based browser.
    
2. Start playing a video (e.g., on YouTube or a local file).
    
3. Open Developer Tools: Press `F12` or `Ctrl+Shift+I`.
    
4. Navigate to the **Media** tab. If you don't see it, click the three dots (`...`) or `>>` (More tabs) on the Developer Tools toolbar, then select `Media`.
    
5. In the "Players" section on the left, click on the entry corresponding to your video.
    
6. In the main panel, scroll down to the **Video Decoder** section.
    
7. Look for the `Hardware decoder` label. It should be `true`. If it says `false` or shows a software decoder name (e.g., `FFmpegVideoDecoder`, `VpxVideoDecoder`, `Dav1dVideoDecoder`), hardware acceleration is not active for that video.
    

## GPU & Browser Configurations

### AMD Radeon RX 6900 XT (Google Chrome)

- **Browser:** Google Chrome
    
- **GPU:** AMD Radeon RX 6900 XT
    
- **Flags File:** `~/.config/chrome-flags.conf`
    

```bash
--use-gl=angle
--use-angle=vulkan
--enable-features=Vulkan,VulkanFromANGLE,DefaultANGLEVulkan,AcceleratedVideoDecodeLinuxZeroCopyGL,AcceleratedVideoEncoder,VaapiIgnoreDriverChecks,UseMultiPlaneFormatForHardwareVideo
--ozone-platform-hint=x11
```

**Notes:** Leverages Vulkan (via ANGLE) and VA-API. `--ozone-platform-hint=x11` can be useful even on Wayland for certain acceleration paths.

---

### Template to contribute

### [Your Browser] - [Your GPU Model] (Contributed by [Your Name/Handle])

- **Browser:** [e.g., Brave, Ungoogled Chromium, Microsoft Edge, Vivaldi, Opera, Chromium]
    
- **GPU:** [e.g., NVIDIA GeForce RTX 3080, Intel Iris Xe]
    
- **Flags File Path:** (Crucial, varies per browser!)
    
    - **Common `.conf` paths:**
        
        - **Chromium:** `~/.config/chromium-flags.conf`
            
        - **Brave Browser:** `~/.config/brave-browser-flags.conf`
            
        - **Ungoogled Chromium:** `~/.config/ungoogled-chromium-flags.conf`
            
    - **`.desktop` file modification:** Some browsers (Brave, Edge, Vivaldi, Opera) might require editing the `Exec=` line in their `.desktop` file (copy from `/usr/share/applications/` to `~/.local/share/applications/` first).
        

**Flags Content (for `.conf` file or `Exec=` line):**

```bash
# Paste your flags here.
# For .desktop files, flags are space-separated after the executable.
```

**Notes (Optional):**

- Required drivers (e.g., `nvidia-dkms`, `intel-media-driver`).
    
- Specific setup considerations or `.desktop` file modification instructions.
