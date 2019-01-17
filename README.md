# How to setup CUDA 10 for Ubuntu 18.04 with RTX 2080 Ti card.

This tutorial is checked on Ubuntu 18.04 Bionic Beaver Linux.
Make sure you have an internet connection.

**NVIDIA driver installation:**
1. Download the display driver for RTX 2080Ti Linux using this [link](https://www.nvidia.com/download/driverResults.aspx/138279/en-us).
2. Open up terminal and enter the following linux commands to install driver.

```sh
    chmod +x ~/Downloads/NVIDIA-Linux-x86_64–410.57.run
    sudo ./NVIDIA-Linux-x86_64–410.57.run
```
3.  Select Continue installation. 
      Registering the kernel module sources with DKMS  - No.
     “Would you like to run nvidia-xconfig utility to automatically update your X configuration file..” - select No

4. Once the drivers are successfully installed, reboot the machine.  To verify the installation, run one of the following commands.

```sh
    nvidia-settings
    nvidia-smi output
```
**Preparing for CUDA installation:**

5. Ubuntu contains a third party open source driver for NVIDIA GPUs called Nouveau. CUDA requires replacing the Nouveau driver with the official closed source NVIDIA driver. So you need to disable default driver before installing CUDA. 

```sh
    sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf”
    sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```

6. Now logout from the GUI with ctrl+alt+f3. Don’t worry, it will be a black screen. Enter your login and password. Switch to root user by using following command.
```sh
    sudo -i 
```

7. After changing to a text console (by pressing Ctrl+Alt+F3) and logging in as root, use the following command to disable the graphical target:
```sh
    systemctl isolate multi-user.target
```
At this point, you would be able to unload the Nvidia drivers using modprobe -r :
```sh
    modprobe -r nvidia-drm
```
7. Update kernel initramfs.
```sh
    update-initramfs -u
```

**CUDA installation:**

8. Now we are ready to install CUDA
```sh
    sudo sh cuda_10.0.130_410.48_linux.run
```

9. When the cuda installation wizard have appeared press ‘Q’ to skip agreement reading and follow installation steps as shown below.
--picture--

10. CUDA 10.0  will be installed on your Ubuntu. 

11. Add cuda path to the PATH variable by running the following.
```sh
    export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    source ~/.bashrc
```

12. Reboot your machine

13. For installation verification open up terminal and run
```sh
    nvcc -V
```

14. Install cuDNN 7.3
Login/register on developer.NVIDIA.com. Download release version and run following command:
```sh
    sudo dpkg -i libcudnn7_7.3.0.29–1+cuda10.0_amd64.deb
```

15. That’s all!  Now you are ready to install your favorite machine learning library with CUDA support.

# Contact
If you have any further questions or suggestions, please, do not hesitate to contact  by email at a.sadenov@gmail.com.

