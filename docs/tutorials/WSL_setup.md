# WSL Setup

## Background
WSL stands for Windows Subsystem Linux. It allows you to run a linux distribution on your Windows machine without having to use 3rd party software such as VirtualBox. This tutorial will walk you through how to setup WSL and get started with Bluetooth. 

## 0. Drivers
The Plugable USB driver isn't installed by default. Plug in the USB, then in Settings go to

  > Windows Update > Advanced options > Optional updates > Driver updates

and if there's something there that says "Broadcom", "Bluetooth" or "plugable", install it and restart your computer. Otherwise, move on.

## 1. Install "USB/IP Device"

First, [install usbipd-win] (https://github.com/dorssel/usbipd-win/releases/tag/v2.4.1) from GitHub, it should be pretty straightforward. Just download the .msi installer and run it. If you want, instead of using the installer you can just do winget install usbipd . It works well.

This is the service that will let us give ownership of the Bluetooth USB to our WSL2, which we have to do since USB devices can only "talk" to one kernel at once.

## 2. Download Kernel, Exported Instance, and Config File

Download from the link below and unzip if the files come zipped.

[https://cornell.app.box.com/s/7e7humgzwu7otqnvikh8m2d07z6pdao5](https://cornell.app.box.com/s/7e7humgzwu7otqnvikh8m2d07z6pdao5)

Move .wslconfig to %UserProfile% (double click on the address bar in Windows File Explorer) and make sure it's named .wslconfig (the leading dot is intentional). %UserProfile$ is very likely C:\Users\<your username>.

You can probably leave the other two files where they are unzipped. Open .wslconfig with a text editor and change the path in the second line to something like

  ```bash
  kernel=C:\\Users\\alexc\\Downloads\\Fast_Robots_WSL_Kernels\\wsl_kernel
  ```
where the path is the path to the unzipped WSL kernel. Double backslashes are needed. Windows also might have unzipped the files into a second folder. i.e. Downloads\\Fast_Robots_WSL_Kernels\\Fast_Robots_WSL_Kernels\\wsl_kernel, so you might want to fix that by moving the inner folder up one folder.

## 2.5. Install (and update) WSL if you haven't already
Run
  ```bash
  wsl --install --no-distribution
  ```
to install the WSL tools without installing the default Linux distribution and

  ```bash
  wsl --update
  ```
to update.

## 3. Import the Fast Robots WSL instance

  ```bash
  wsl --import FastRobots $env:UserProfile\FastRobotsWSL $env:UserProfile\Downloads\Fast_Robots_WSL_Kernels\FastRobotsWSL_V0.tar
  ```
## 4. Start Bluetooth

These next few commands, you'll probably have to **run every time you restart**, or whenever it stops working.

Open a **Windows** PowerShell window as **administrator** (for the first time connecting the Bluetooth USB adapter or as necessary), then run:
  ```bash
  usbipd wsl list
  ```

This will give you a list of bus devices, we're interested in the one that says something about "Broadcom Bluetooth USB Device". Take its BUSID from the leftmost column and run:
  ```bash
  usbipd wsl attach --busid=<BUSID>
  ```
Then, open up your **WSL2** terminal and run:
  ```bash
  sudo service dbus start
  sudo service bluetooth start
  ```
You may then activate the python3 venv provided in fastrobots' home directory (in WSL terminal)

  ```bash
  source ~/FastRobots_ble/bin/activate
  ```
and open Jupyter Lab wherever you have your lab notebook. My Windows document directory, for example, could be changed to with
  
  ```bash
  cd /mnt/c/Users/alexc/Documents
  ```

Jupyter will give you a link that you can use in your Windows web browser.





