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
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 26.04.1 LTS
Release:        26.04
Codename:       resolute
```
in this tutorial we would use the **Lyrical Luth**  as demo example.
> [!CAUTION]
> install the wrong ros distro who dont support the **EXACT** version may lead to compatability issue.
### getting started
run the following command first.

Assumed that you have `apt` `curl` tools. if not , please manually install them.
```bash
locale  # check for UTF-8
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale  # verify settings
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
sudo apt update
sudo apt install ros-dev-tools # dev tool , to build the bin of executable of ros.
sudo apt install ros-lyrical-desktop # for Desktop install (Recommended): ROS, RViz, demos, tutorials.
sudo apt install ros-lyrical-ros-gz
```
verify the aforementioned script by the following instruction.



