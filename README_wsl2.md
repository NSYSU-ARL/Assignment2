## Distro Setup and tested enviroment
- OS : win11 25H2
- GPU : RTX5060

> [!NOTE]
> if you havent install the distro on windows , run the following command in powershell to install.
> ```bash
> wsl --install -d Ubuntu-22.04
> ```
## install ROS2
the following is the summary of the docs 
https://www.ros.org/blog/getting-started/#
### distro of ROS2
disro means the variant of the ROS2

- ROS 2 **Lyrical Luth** is the latest ROS 2 long-term release. It supports **Ubuntu 26.04** and Windows 11.
- ROS 2 **Jazzy Jalisco** is an active ROS 2 long-term release. It supports **Ubuntu Linux 24.04** and Windows 10.
- ROS 2 **Humble Hawksbill** is the best upgrade path for ROS 1 users. It supports **Ubuntu Linux 22.04** and Windows 10.

since the lastest and the default version of the ubuntu on `wsl2` is 26.x , we recommend use **Lyrical Luth** as the distro of the ros.

you can use the following command to inspect the version of your wsl2 distro.
```bash
~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.5 LTS
Release:        22.04
Codename:       jammy
```
make sure the version is 22.x

in this tutorial we would use the **Lyrical Luth**  as demo example.
> [!CAUTION]
> install the wrong ros distro who dont support the **EXACT** version may lead to compatability issue.
### getting started
run the following script to setup and install the dependency and the enviroment
```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt install ros-humble-desktop
sudo apt install ros-dev-tools
source /opt/ros/humble/setup.bash

```
using the `ros2` command to verfiy the installation
```text
~$ ros2 | head -n 2
usage: ros2 [-h] [--use-python-default-buffering]
            Call `ros2 <command> -h` for more detailed usage. ...
```

## run the simple example in the repo

```bash
git clone https://github.com/NSYSU-ARL/Assignment2.git
```
after clone `cd` into the repo folder , create the `src` folder to align with the script example of the repo.
```bash
seans@MSI:~/Assignment2$ mkdir src
seans@MSI:~/Assignment2$ mv nsysu* ./src
```
verify if the critical enviroment varialbe is exist (if you have install this.)
```text
$ echo $ROS_DISTRO
humble
```
if not , run the following command
```bash
source /opt/ros/humble/setup.bash
```
init and update the rosdep program
```bash
sudo rosdep init
rosdep update
```



