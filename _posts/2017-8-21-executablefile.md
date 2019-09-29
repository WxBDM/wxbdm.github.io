---
layout: post
title: Creating a Python executable file using PyInstaller
---

This post goes over creating an executable file for your Python project or script.

### Installing

First, check to make sure you have PyInstaller: `$pip freeze`. If you don't, use pip to install: `$pip install pyinstaller`

### Creating the executable file

Now, navigate to where your file is. Suppose mine is on the desktop: `$cd /Users/Brandon/Desktop/myExecutable.py`

Now, use PyInstaller to create the executable file: `$pyinstaller --onefile myExecutable.py`

After about 5 seconds or so, you should have 3 new folders and 2 files in the given directory. Your executable file is under the `dist` folder.

