# PyAutoGUI Installation Guide

## Issue
PyAutoGUI installation is currently failing on Linux systems with Python 3.13 due to build issues. The error typically shows:
```
ERROR: Could not install packages due to an OSError: [Errno 2] No such file or directory: '/tmp/tmp*/output.json'
```

## Scripts Using PyAutoGUI

1. **`src/agents/code_runner_agent.py`** - macOS-only (uses Quartz/AppKit)
2. **`src/agents/tiktok_agent.py`** - macOS-only (uses Quartz/AppKit)
3. **`src/scripts/find_coordinates.py`** - Cross-platform (can work on Linux)

## Manual Installation Options

### Option 1: Install System Dependencies First (Linux)
```bash
# On Arch/Manjaro
sudo pacman -S python-xlib scrot python-tkinter

# Then try installing pyautogui
pip install pyautogui
```

### Option 2: Install from Source with Build Isolation Disabled
```bash
pip install --no-build-isolation pyautogui
```

### Option 3: Use System Python Instead of Venv
```bash
# Try installing with system Python
python3 -m pip install --user pyautogui
```

### Option 4: Install Older Version
```bash
pip install pyautogui==0.9.50
```

### Option 5: Install Pre-built Wheel (if available)
```bash
pip install --only-binary :all: pyautogui
```

## Alternative Libraries for Linux

If pyautogui continues to fail, consider these alternatives:

1. **`pynput`** - Cross-platform input control
   ```bash
   pip install pynput
   ```

2. **`keyboard`** - Keyboard automation
   ```bash
   pip install keyboard
   ```

3. **`mouse`** - Mouse automation
   ```bash
   pip install mouse
   ```

## Notes

- `code_runner_agent.py` and `tiktok_agent.py` are macOS-specific and won't work on Linux even if pyautogui is installed (they require Quartz/AppKit frameworks)
- `find_coordinates.py` can work on Linux if pyautogui is successfully installed
- The build error appears to be related to Python 3.13 compatibility issues with pyautogui's build system

## Current Status

- ✅ Scripts updated to detect platform and provide helpful error messages
- ❌ PyAutoGUI installation failing due to build system issues
- ✅ Alternative solutions documented above

