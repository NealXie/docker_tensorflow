## Docker + TensorFlow Environment Configuration

### What is Docker?
Docker is the world's leading software container platform. Developers use Docker to eliminate "works on my machine" problems when collaborating on code with co-workers. Operators use Docker to run and manage apps side-byside in isolated containers to get better compute density. Enterprises use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux, Windows Server, and Linux-on-mainframe apps.

### What is a Docker Container?
Docker containers are a way to package software in a format that can run isolated on a shared operating system. Unlike VMs, containers do not bundle a full operating system - only libraries and settings required to make the software work are needed. This makes for efficient, lightweight, self-contained systems and guarantees that software will always run the same.

### What is TensorFlow?
TensorFlow is an open source sofware library for numerical computation using data flow graphs. Nodes in the graph represent mathematical operations, while the graph edges represent the multidimensional data arrays (tensors) communicated between them. The flexible architecture allows you to deploy computation to one or more CPUs or GPUs in a desktop, server, or mobile device with a single API. TensorFlow was originally developed by researchers and engineers working on the Google Brain Team within Google's Machine Intelligence research organization for the purposes of conducting machine mearning and deep neural networks research, but the system is general enough to be applicable in a wide variety of other domains as well.

### Why Docker + TensorFlow?
Businesses ike fast, data-driven insights, and they employ data scientists to make them. Practicing data science is an exploratory, iterative process requiring los of computing resources and lots of time. To better support exploratory iteration, data scientists often use notebooks like Jupyter, and to accelerate computation of TensorFlow jobs they're increasingly using to GPUs.
There is currently a trend in cloud computing to use Kubernetes and Docker o improve resource utilization. Wouldn't it be bgreat if data science tools like jupyter and GPUs could be managed with Docker and Kubernetes? It would enable saving time and money.

### Operating System - Ubuntu 16.04.3 LTS (Xenial)
This document will use Ubuntu as an example to introduce you how to set up the whole environment of Docker and TensorFlow. Mac will be very similar to this, but Windows not. Since the Linux system is much better on general programming due to abundance of resources, I would highly suggest to use Linux based system to do such operating and environment setting. System info as below:
```bash
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.3 LTS
Release:        16.04
Codename:       xenial
```

### Determin which TensorFlow to install
There are two types of TensorFlow you can choose to install:  
* **TensorFlow with CPU support only.** If your system does not have a NVIDIA GPU, you must install this version. This version of TensorFlow is typically much easier to install, so even if you have an NVIDIA GPU, I will recommend installing this version first. This document will focus on this version so you can get a baseline and get interest of this library.  

* **TensorFlow with GPU support.** TensorFlow programs typically run sginificantly faster on a GPU than on a CPU. Therefore, if your system has a NVIDIA GPU and you need to run performance-critical applications, you should go to TensorFlow website to check you prerequisites and install this version.  

### Get Docker CE for Ubuntu  
For Docker, you need the Docker CE version. Docker CE is supported on Ubuntu on `x86_64`, `armhf`, and `s390x` architectures.  

#### Uninstall old versions
Older versions of Docker were called `docker` or `docker-engine`. If these are installed, uninstall them first:  
```bash
$ sudo apt-get remove docker docker-engine docker.io
```
It's OK if `apt-get` reports that none of these packages are installed.  

#### Install Docker CE
You can install Docker CE in different ways, depending on your needs. Most users set up Docker's repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach. This document will use this approach.  
