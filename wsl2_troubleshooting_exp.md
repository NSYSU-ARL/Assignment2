## Distro Setup and Tested Environment
- OS: Windows 11 25H2
- GPU: RTX 5060

> [!NOTE]
> If you have not installed the distro on Windows yet, run the following command in PowerShell:
>
> ```bash
> wsl --install -d Ubuntu-22.04
> ```

## Install ROS 2
The following is a summary of the official ROS documentation:
https://www.ros.org/blog/getting-started/

### ROS 2 Distros
"Distro" refers to the variant of ROS 2.

- ROS 2 Lyrical Luth is the latest long-term support (LTS) release. It supports Ubuntu 26.04 and Windows 11.
- ROS 2 Jazzy Jalisco is an active LTS release. It supports Ubuntu Linux 24.04 and Windows 10.
- ROS 2 Humble Hawksbill is the best upgrade path for ROS 1 users. It supports Ubuntu Linux 22.04 and Windows 10.

You can check the version of your WSL2 distro with the following command:

```bash
lsb_release -a
```

Example output:

```text
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.5 LTS
Release:        22.04
Codename:       jammy
```

Make sure the version matches the demo environment on this page (22.x).

In this tutorial, we will use the ROS 2 Humble distribution as the example.

> [!CAUTION]
> - Installing the wrong ROS 2 distro that does not match the exact Ubuntu version may lead to compatibility issues.
> - We strongly recommend using the Humble distro for full compatibility with Gazebo Classic.
> - Using the latest Gazebo version on the latest ROS 2 release may also cause compatibility issues.

### Getting Started
Official installation guide:
https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html

Run the following script to set up the required dependencies and environment:

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

Verify the installation using the `ros2` command:

```text
~$ ros2 | head -n 2
usage: ros2 [-h] [--use-python-default-buffering]
            Call `ros2 <command> -h` for more detailed usage. ...
```

## Run the Simple Example in the Repository

```bash
git clone https://github.com/NSYSU-ARL/Assignment2.git
```

After cloning, move into the repository folder and create a `src` directory to match the workspace structure used by the project scripts:

```bash
cd Assignment2
mkdir src
mv nsysu* ./src
```

Verify that the required environment variable exists (if it has already been installed):

```text
$ echo $ROS_DISTRO
humble
```

If it does not exist, run:

```bash
source /opt/ros/humble/setup.bash
```

Initialize and update `rosdep`:

```bash
sudo rosdep init
rosdep update
rosdep install -r -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

Build the executable:

```bash
colcon build --packages-select-regex nsysu.*
```

The build may take some time.

After the build completes, the repository folder should look similar to the following:

```text
$ ls
build  Dockerfile  install  LICENSE  log  README.md  run_docker.sh  src
```

- `install` is the workspace used by the ROS 2 system.

Activate the build:

```bash
source install/setup.bash
```

Start the main test program:

```bash
ros2 launch nsysu_drone_bringup nsysu_drone_bringup.launch.py
```

<img width="958" height="508" alt="image" src="https://github.com/user-attachments/assets/7faec186-066a-44f3-b306-fff2a86eaf27" />

This is the RViz program, which represents the sensor data received from the drone.

---

<img width="958" height="508" alt="image" src="https://github.com/user-attachments/assets/b2aa1943-651a-4988-9c47-a589481f39a3" />

This is the Gazebo Classic view.

If the FPS is extremely poor, inspect the logs to confirm whether the view is being rendered using a GPU-accelerated path or falling back to software rendering.

```bash
sudo apt install mesa-utils
```

Install the tools needed to inspect the rendering logs:

Run the log inspector script as follows:

```bash
glxinfo -B | grep -i "renderer"
```

Example output:

```text
Extended renderer info (GLX_MESA_query_renderer):
OpenGL renderer string: D3D12 (Intel(R) UHD Graphics)
```

> [!IMPORTANT]
> If the output includes `llvmpipe (LLVM 15.0.7, 256 bits)`, this indicates that the system is falling back to CPU rendering.

> [!NOTE]
> Unfortunately, I was unable to reproduce the buggy environment where the renderer falls back to CPU simulation. However, the following environment variables may still be useful as a reference:
>
> ```bash
> echo "export GALLIUM_DRIVER=d3d12" >> ~/.bashrc
> echo "export QT_X11_NO_MITSHM=1" >> ~/.bashrc
> source ~/.bashrc
> ```

