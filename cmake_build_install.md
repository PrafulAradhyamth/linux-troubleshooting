# **Global Build & Install Script Setup**

This guide explains how to create a **Bash script** that automates the typical CMake build process:

```bash
mkdir build
cd build
cmake ..
make -j8
sudo make install
sudo ldconfig
```

Once configured, you can call the script **from anywhere** using a single command.

---

## **1. Create the Script**

Open a terminal and create a new file:

```bash
nano ~/build_install.sh
```

Paste the following content:

```bash
#!/bin/bash
set -e  # Exit immediately if any command fails

# Create and enter build directory
mkdir -p build
cd build

# Run CMake, compile, install, and refresh linker cache
cmake ..
make -j$(nproc)
sudo make install
sudo ldconfig

echo "Build and installation completed successfully!"
```

Save and exit:

* Press **Ctrl + O** → **Enter**
* Press **Ctrl + X** to close.

---

## **2. Make the Script Executable**

```bash
chmod +x ~/build_install.sh
```

---

## **3. Move Script to a Global Path**

Move the script to `/usr/local/bin` so it can be run **from anywhere**:

```bash
sudo mv ~/build_install.sh /usr/local/bin/build_install
```

---

## **4. Verify Installation**

Check if the script is available globally:

```bash
which build_install
```

Expected output:

```
/usr/local/bin/build_install
```

---

## **5. Usage**

From **any directory**, simply run:

```bash
build_install
```

This will:

1. Create a `build/` folder in the **current directory**
2. Run `cmake ..`
3. Compile with `make`
4. Install with `sudo make install`
5. Refresh the dynamic linker cache

---

## **6. (Optional) Add Clean Build Support**

If you frequently rebuild, you can make the script smarter by adding a prompt to clean old builds:

```bash
#!/bin/bash
set -e  # Exit on any error

# If a build directory already exists, ask before removing it
if [ -d "build" ]; then
    read -p "'build' directory exists. Clean and rebuild? [y/N]: " choice
    if [[ "$choice" =~ ^[Yy]$ ]]; then
        rm -rf build
    fi
fi

mkdir -p build
cd build

cmake ..
make -j$(nproc)
sudo make install
sudo ldconfig

echo "Build and installation completed successfully!"
```

---

## **7. Summary Table**

| **Step**        | **Command**                                               | **Description**             |
| --------------- | --------------------------------------------------------- | --------------------------- |
| Create script   | `nano ~/build_install.sh`                                 | Write the build script      |
| Make executable | `chmod +x ~/build_install.sh`                             | Allow execution             |
| Move globally   | `sudo mv ~/build_install.sh /usr/local/bin/build_install` | Add script to system path   |
| Verify          | `which build_install`                                     | Check if available globally |
| Run             | `build_install`                                           | Start the build process     |

---

## **8. Final Result**

After following this guide, you can run:

```bash
build_install
```

from **anywhere** in your system, and it will handle the full build and installation process automatically. 


