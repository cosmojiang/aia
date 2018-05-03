## Install Tensorflow on Ubuntu 18.04 LTS

This instruction documents what I did to install Tensorflow and necessary dependencies on Ubuntu 18.04 LTS. Contents are mostly inspired by the post https://medium.com/@taylordenouden/installing-tensorflow-gpu-on-ubuntu-18-04-89a142325138, but with some modifications that ensures the successful installation. 

**Ubuntu 18.04 installation**
Please follow the official installation guideline or the page here https://linuxconfig.org/how-to-install-ubuntu-18-04-bionic-beaver

**NVIDIA driver installation**
Check available version of NVIDIA drivers and install a proper version (I used the 390). 
```shell
$ ubuntu-drivers devices
$ sudo apt install nvidia-driver-390
```
Reboot your computer (very important) and check your driver version
```shell
$ nvidia-smi
```
You should see information like this.
<p align="left">
  <img src="doc/img/nvidia_driver.png" width=140 height=195>
</p>

**CUDA9 installation**
Unlike the post, I successfully installed the CUDA9 using the deb file, which is somehow more straightforward than runfile.

Download the CUDA9 toolkit and update patches (for Ubuntu 17.04) throught the official website https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1704&target_type=deblocal

After downloading, direct to the folder where you save those files.
Use the following commands to install the CUDA9 toolkit
```shell
$ sudo dpkg -i cuda-repo-ubuntu1704-9-0-local_9.0.176-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
$ sudo apt-get update
$ sudo apt-get install cuda
```
Use the Ubuntu software installer to install the two update patches (Double-click them as you did on Windows).

