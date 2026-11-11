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



you can use the following command to inspect the version of your wsl2 distro.
```bash
~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.5 LTS
Release:        22.04
Codename:       jammy
```
make sure the version is aligned with the demo om this page (22.x)

in this tutorial we would use the **HUMBLE**  as demo example.
> [!CAUTION]
> - install the wrong ros distro who dont support the **EXACT** matched ubuntu version  may lead to compatability issue.
> 
> - strongly recommend to use the **HUMBLE** distro for the full compatability with the gazebo classic.
> 
> - using the lastest version of gazebo on the latest version of the ROS2 may lead to compatability issue.
### getting started
https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html

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
rosdep install -r -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```
build the executable
```bash
colcon build --packages-select-regex nsysu.*
```
the build process may need a while.

after running the build command , the file in the repo folder would be like the following.
```text
$ ls
build  Dockerfile  install  LICENSE  log  README.md  run_docker.sh  src
```
- where the `install` is the workspace for the `ros2` system
activate the build
```bash
source install/setup.bash
```
activate the main test program
```bash
ros2 launch nsysu_drone_bringup nsysu_drone_bringup.launch.py
```
<img width="958" height="508" alt="image" src="https://github.com/user-attachments/assets/7faec186-066a-44f3-b306-fff2a86eaf27" />

this is the ritz program , which represnet the data received from the sensor on the drone

---

<img width="958" height="508" alt="image" src="https://github.com/user-attachments/assets/b2aa1943-651a-4938-9c47-a589481f39a3" />

this is the view of the gazebo classic  , 

if the FPS is extreme poor ,consider inspect the log to verify if the view is rendered by the GPU-like approach or not.

```bash
sudo apt install mesa-utils
```

install the tools to inspect the logs of the rendering component

run the log inspector script as follows
 ```bash
 $ glxinfo -B | grep -i "renderer"
 Extended renderer info (GLX_MESA_query_renderer):
 OpenGL renderer string: D3D12 (Intel(R) UHD Graphics)
```
>  [!IMPORTANT]
> if the grepped text include the `llvmpipe (LLVM 15.0.7, 256 bits)` , which means the fallback to the cpu.

> [!NOTE]
>Unfortunately , I failed to reprocude the buggy enviroment where the renderer failback to the cpu simulation .
>however , the following enviroment variable can still be the refernece 
>```bash
>echo "export GALLIUM_DRIVER=d3d12" >> ~/.bashrc
>echo "export QT_X11_NO_MITSHM=1" >> ~/.bashrc
>source ~/.bashrc
> ```


