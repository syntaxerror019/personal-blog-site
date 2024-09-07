---
title: "Using OpenCV on RPI in 2024"
date: 2024-07-31T13:03:04-04:00
draft: false
excerpt: "Who's right? A working way to install cv2 on RPI in 2024"
tags: ["RPI", "CV2", "opencv", "tutorial"]
featured: false
---

---

## Introduction
Between all the various tutorials online, there is no straight way to installing OpenCV on a RPI...  

Issue's I have encountered personally include but are not limited to the following:

- Building from source issues
- Various rare errors
- Garbage tutorials

So in this simple guide, I have documented my installation of OpenCV on my Raspberry Pi, Model 3 B+  

Other models should be quite similar.

---

## Requirements
For this project, I used a Raspberry Pi 3 B+ running Ubuntu Server 20.04  

I've stopped using the Raspbian image on my pi for numerous reasons, mainly due to the smaller community base and lack of support for software when compared to Ubuntu. I also use Ubuntu daily, and I am very familiar with it.  

You will also need an internet connection, a way to connect to your RPI (be it SSH, USB, VNC, etc.), and an SD card with some reasonable levels of (free) storage  

---

## Getting Started
To begin, you should always start with the classic
```bash
sudo apt-get update
sudo apt-get upgrade
```  

and once everything is up to date, you can install the necessary packages for OpenCV:
```bash
sudo apt-get install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev python3-dev
```

----

## Instlling pip Package Manager
If you have not already done so, you can install pip the package manager for python using this command:
```bash
sudo apt-get install python3-pip
```

and once that finishes, you can use it to install Numpy:
```bash
pip3 install numpy
```

---

## Getting OpenCV
Almost done! Now you can just install OpenCV. Before you start trying to install though, it is important you install the correct version.  
If your RPI is running Ubuntu Server (or similar) and has no GUI (e.g. boots into a terminal screen with no GUI) you will want to install a different version than if your RPI has a GUI.  

If you don't have a GUI run the following command. This will omit GUI functionality.
```bash
pip3 install opencv-python-headless
```  

If you have a GUI on your RPI, you can try installing the normal OpenCV version.
```bash
pip3 install opencv-python
```  

**Note:** If you have a GUI and the GUI installation doesn't work (or importing cv2 in python throws an error) it may be worth trying the headless version. Be sure you use pip3 to uninstall the existing version before trying to install a different one!  

--- 

## Importing and Testing
If the installation (which may take a while depending on hardware and internet speed) completed without error, now is the time to test everything!

```bash
python3
```
```bash
>>> import cv2
>>> print(cv2.__version__)
```

if the basic `import cv2` throws an error, read the note on the above step and try again.  
If you still can't figure it out, leave a comment!

---

## Wrapping Up
You have now installed OpenCV on your RPI running Ubuntu (or similar OS) congratulations! 🎉

If you have any questions or comments, please let me know in the comment section below and I'll be happy to help you out if you provide your situation. If you would prefer to email me (more direct and reliable contact) shoot me an email at `sntx@sntx.dev` and I'll get back to you shortly!  

Thank you!