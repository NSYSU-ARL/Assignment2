## Distro Setup and tested enviroment
- OS : win11 25H2
- GPU : RTX5060
- 
if you havent install the distro on windows , run the following command in powershell to install.
```bash
wsl --install -d Ubuntu-22.04
```
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
### getting started
