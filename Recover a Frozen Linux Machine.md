# How to Recover a Frozen Linux Machine

When your Linux system freezes, the solution depends on **what exactly froze** — the GUI, the terminal, or the entire kernel.
This guide walks you through solutions from **safe** → **forceful**.

---

## **1. Check if Only the GUI is Frozen**

Try switching to a **TTY (virtual console)**:

```bash
Ctrl + Alt + F2
```

or

```bash
Ctrl + Alt + F3
```

* If successful, you’ll see a login prompt.
* Log in and find problematic processes:

```bash
top
```

* Kill the offending process:

```bash
kill -9 <PID>
```

* Or reboot gracefully:

```bash
sudo reboot
```

---

## **2. Restart the GUI (Display Manager)**

If you can access a TTY, restart only the GUI instead of rebooting:

* **GNOME** (Ubuntu, Fedora, Debian):

  ```bash
  sudo systemctl restart gdm
  ```
* **KDE Plasma**:

  ```bash
  sudo systemctl restart sddm
  ```
* **LightDM** (older Ubuntu, Xfce, etc.):

  ```bash
  sudo systemctl restart lightdm
  ```

---

## **3. Use Magic SysRq (Kernel-Level Escape Hatch)**

If **even TTYs don't work**, try the **Magic SysRq key** — a direct interface with the Linux kernel.

### **Steps**

Hold **`Alt` + `SysRq` (Print Screen)** and press the following keys **one by one, slowly**:

```
R  E  I  S  U  B
```

### **What It Does**

| Key | Action                                |
| --- | ------------------------------------- |
| R   | Switch keyboard to raw mode           |
| E   | Send `SIGTERM` to processes           |
| I   | Send `SIGKILL` to remaining processes |
| S   | Sync disks                            |
| U   | Unmount disks                         |
| B   | Reboot immediately                    |

> **Mnemonic**: **"Reboot Even If System Utterly Broken"** 
> To **shutdown instead of rebooting**, replace **B** with **O**.

---

## **4. Hard Power Cycle (Last Resort)**

If none of the above works:

1. Hold the **power button** for **5–10 seconds** until the system powers off.
2. Boot back up normally.

---

## **5. Diagnose Why It Froze**

After rebooting, inspect logs to prevent future freezes:

```bash
journalctl -xe
```

or:

```bash
dmesg | tail -n 50
```

Check for:

* GPU driver issues
* Out-of-memory kills
* Kernel panics
* High CPU or I/O load

---

## **Summary Cheat Sheet**

| Situation             | Solution                                       |
| --------------------- | ---------------------------------------------- |
| GUI frozen            | `Ctrl + Alt + F2` → kill process / reboot      |
| Restart GUI           | `sudo systemctl restart gdm` (or sddm/lightdm) |
| System totally frozen | Use **Magic SysRq** → `REISUB`                 |
| Absolute last resort  | Hold **power button**                          |


