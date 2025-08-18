
# Linux Troubleshooting Guide 

A collection of Linux/Ubuntu troubleshooting guides, configurations, and tips.  
This repository is intended as a personal reference, but contributions and suggestions are welcome.

---

##  Repository Structure

```

linux-troubleshooting/
├── README.md                 # Overview of the repo
├── ubuntu/
│   ├── headless-display.md   # Guide: Headless display / blank screen issues
│   ├── gdm-lightdm.md        # Guide: Switching between GDM3 and LightDM
│   ├── gpu-drivers.md        # GPU driver installation and fixes
│   ├── system-logs.md        # How to check system logs and errors
│   └── fixes/                # Config snippets (Xorg, dummy driver, etc.)
├── other-distros/            # Guides for Fedora, Arch, etc. (future)
└── assets/                   # Screenshots, diagrams, flowcharts

```

---

##  Ubuntu Guides

### 1. Headless Display / Blank Screen
- When the system boots with no monitor detected.
- Includes:
  - Disabling Wayland (`WaylandEnable=false`)
  - Persistent Xorg monitor config
  - Dummy driver for forced resolution
  - ASCII flowchart for quick troubleshooting
- File: [`ubuntu/headless-display.md`](ubuntu/headless-display.md)

### 2. GDM / LightDM Switching
- How to switch between display managers
- Fixes for inactive display managers
- File: [`ubuntu/gdm-lightdm.md`](ubuntu/gdm-lightdm.md)

### 3. GPU Drivers & Issues
- Installing NVIDIA, AMD, Intel drivers
- Common boot-time GPU errors and fixes
- File: [`ubuntu/gpu-drivers.md`](ubuntu/gpu-drivers.md)

### 4. System Logs
- Commands to check logs for boot, GPU, display manager, and services
- File: [`ubuntu/system-logs.md`](ubuntu/system-logs.md)

---

##  How to Use
1. Navigate to the folder corresponding to your Linux distribution.  
2. Read the Markdown guides for step-by-step instructions.  
3. Config snippets are in `ubuntu/fixes/`. Copy them to `/etc/X11/xorg.conf.d/` or as instructed.  
4. Screenshots and diagrams are in `assets/` for visual guidance.

---

## Contribution
- Contributions, additional guides, or corrections are welcome via Pull Requests.  
- Keep each guide self-contained and update the repo structure if adding new distro or config.

---

## References
- [Ubuntu Official Documentation](https://help.ubuntu.com/)
- [Xorg Configuration](https://wiki.archlinux.org/title/xorg)
- [Systemd Journal](https://www.freedesktop.org/software/systemd/man/journalctl.html)

---


