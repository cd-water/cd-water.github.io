# **从零开始，使用VMware搭建CentOS 7 Linux虚拟机**

**摘要：本文是一份面向新手的详尽指南，将一步步演示如何使用VMware Workstation Pro这款强大的虚拟机软件，成功安装并配置一个纯净的CentOS 7 Linux操作系统。无论你是为了学习Linux、搭建开发环境，还是进行软件测试，本文都将为你提供一个稳定、隔离的练习平台。**


## 一、安装虚拟机软件**VMwareWorkstationPro**

安装Linux操作系统有以下几种方法：

* 物理机安装

**物理机安装**意味着将Linux系统直接写入电脑的硬盘中，它可以完全独享计算机的所有硬件资源，因此能提供最佳的性能和最纯粹的使用体验。然而大多数人的主流操作系统为Windows操作系统，可能对用户的办公造成不便。

* WSL

**Windows子系统Linux（WSL）** 是微软提供的一个非常轻便的解决方案。它并非一个完整的虚拟电脑，而是一个与Windows深度集成的兼容层，允许用户直接在Windows内部运行Linux命令行工具。它的优点是极其轻量、启动迅速，并且可以无缝访问Windows文件系统。更适合需要在Windows环境下偶尔使用Linux命令的开发者，而非想要系统学习Linux整体运作的初学者。

* 虚拟机安装

**虚拟机安装**通过VMware这类专业软件，在当前使用的Windows或macOS系统（称为宿主机）上模拟出一台完整的“虚拟电脑”。您可以将Linux系统像安装一个普通软件一样安装在这台虚拟电脑里。提供了完美的**安全隔离**环境，您在虚拟机内的任何操作，无论是误删系统文件还是测试危险命令，都不会对您真正的电脑系统和数据造成任何影响，适合Linux初学者进行学习。



VMwareWorkstationPro可前往联想应用商店点击下载即可，打开软件页面如下：

<img width="1560" height="1034" alt="Image" src="https://github.com/user-attachments/assets/07112f5b-e51f-41f0-a551-a1d6f8d4df06" />



## 二、在VMwareWorkstationPro中安装CentOS 7

**1.获取CentOS 7镜像文件**

推荐访问国内镜像站，如阿里云镜像进行获取

[[centos-7.9.2009-isos-x86_64安装包下载_开源镜像站-阿里云](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)

**2.新建虚拟机**

<img width="1560" height="1034" alt="Image" src="https://github.com/user-attachments/assets/ba0daece-e860-4acd-877a-34c134058928" />

**3.选择典型**

<img width="927" height="884" alt="Image" src="https://github.com/user-attachments/assets/7953eac8-c9fc-45db-8805-8cf2aa7742e2" />

**4.选择获取的iso镜像文件**

<img width="927" height="884" alt="Image" src="https://github.com/user-attachments/assets/6ac6c526-2fc4-4191-bcfc-f212e4ce6c60" />

**5.设置安装路径**

<img width="927" height="884" alt="Image" src="https://github.com/user-attachments/assets/ab7ca9be-7762-4e23-9bce-f4be191f4cf5" />

**6.设置硬件配置**

<img width="927" height="884" alt="Image" src="https://github.com/user-attachments/assets/ed3996d9-f599-4fcb-bb0e-5247566b65da" />

<img width="927" height="884" alt="Image" src="https://github.com/user-attachments/assets/57cfe68b-beb3-440e-9664-6bc20ae9224e" />

<img width="1381" height="629" alt="Image" src="https://github.com/user-attachments/assets/1ec3be68-7664-4674-bf0f-e31b032ad810" />

**7.安装CentOS 7**

<img width="640" height="480" alt="Image" src="https://github.com/user-attachments/assets/605b7bcc-cf21-4a7a-9987-d738e90003e3" />

**中间过程一系列如语言选择、设置用户等操作根据提示进行即可。**

**8.安装完成**

<img width="1052" height="767" alt="Image" src="https://github.com/user-attachments/assets/a90cec29-1180-4731-9eb4-945da506f445" />