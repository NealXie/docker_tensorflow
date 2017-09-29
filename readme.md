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

Before you install Docker CE for the first time on a new host machine, you need  to set up the Docker repository. Afterward, you can install and update Docker from the repository.  

1. Update the `apt` package index:
```bash
$ sudo apt-get update
```

2. install packages to allow `apt` to use a repository over HTTPS:
```bash
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

3. Add Docker's official GPG key:
```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Verify that you now have the key with the fingerprint:  
`9DC8 582 9FC7 DD38 854A E2d8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.  
```bash
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

4. Use the following command to set up the **stable** reposity. You always need the **stable** repository, even if you want to install builds from the **edge** or **testing** repositories as well. Below is the code for amd64.
```bash
$ sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

5. Install the lastest version of Docker CE. Any existing installation of Docker is replaced.
```bash
$ sudo apt-get install docker-ce
```

6. Verify that Docker CE is installed correctly by running the `hello-world` image.
```bash
$ sudo docker run hello-world
```
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.  

Docker CE is installed and running. Now you can use `sudo apt-get update` to upgrade Docker CE later.

#### Manage Docker as a non-root user
The `docker` daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user `root` and other users can only access it using `sudo`. The `docker` daemon always runs as the `root` user.

If you don't want to use `sudo` when you use the `docker` command, create a Unix group called `docker` and add users to it. When the `docker` daemon starts, it makes the ownership of the Unix socket read/writable by the `docker` group.

1. Create the `docker` group.
```bash
$ sudo groupadd docker
```

2. Add your user to the `docker` group.
```bash
$ sudo usermod -aG docker $USER
```

3. Log out and log back in so that your group membership is re-evaluated.
If testing on a virtual machine, it may be necessary to restart the virtual machine for changes to take affect. On a desktop Linux environment such as X Windows, log out of your session completely and then log back in.

4. Verify that your group info and run `docker` commands without `sudo`.
```bash
$ cat /etc/group
$ docker run hello-world
```

### Install TensorFlow with Docker
This step I will launch a Docker container that contains one of the TensorFlow binary images. To launch a Docker container with CPU-only support (that is, without GPU support), enter a command of the following format:
```bash
$ docker run -it -p hostPort:containerPort TensorFlowCPUImage
```
where:  
* -p hostPort:containerPort is optional. If you plan to run TensorFlow programs from the shell, omit this option. If you plan to run TensorFlow programs as Jupyter notebooks, see both hostPort and containerPort to 8888. If you'd like to run TendorBoard inside the container, add a second -p flag, setting both hostPort and containerPort to 6006.
* TensorFlowCPUImages is required. It idenifies the Docker container. There is a lot of images you can find from Docker Hub.

Now use command to run Docker and download TensorFlow image.
```bash
$ docker run -it -p 8888:8888 tensorflow/tensorflow
```

If you want to share documnets with your container, you can specify your local directory.
```bash
$ docker run -it -p 8888:8888 -v /$(pwd)/notebooks:/notebooks tensorflow/tensorflow
```
You can save your files to local. `/$(pwd)/notebooks` is a local directory, `/noteboos` is a directory of your container. They have been mapped together.

If everything goes well, you can open your terminal link, use browser to access Jpuyter Notebook. In addition, you will find your new create file will also be saved at your local directory.

#### Key Docker Commands
There are some key commands you need to remember when you are running a docker.
1. Check image info.  
`$ docker images`
2. Remove image.  
`$ docker rmi <image name or tag or ID>`
3. Check container info.  
`$ docker ps -a`
4. Remove container.  
`$ docker rm <container name or tag or ID>`
5. Remove all container.  
`$ docker rm $(docker ps -a -q)`
6. Stop a running container.  
`$ docker stop <container name or tag or ID>`
7. Start a stoped container.  
`$ docker start <container name or tag or ID>`

#### Create new docker image
After above steps, you may ask why we need to create a new docker image. The reason is that we want to be more effienct to use TensorFlow. The original image only support python2 and there is no other usefull packages. What I would like to do is to use Tensorflow + Python3 + Jupyter Notebook + tflearn.

Let's use `dash00/tensorflow-python3-jupyter` as a foundation, then add `tflearn` package, finally create a new docker image `neal3153/tensorflow`.

##### Download Image
First, we need to get `dash00/tensorflow-python3-jupyter` image.
```bash
$ docker pull dash00/tensorflow-python3-jupyter 
```
This image includes:
```bash
- Jupyter Notebook
- TensorFlow
- scikit-learn
- pandas
- matpltlib
- scipy
- Pillow
- Python 2 and 3
```

##### Use persistent folder
Second, we need to run this image using the method we mentioned above.
```bash
$ docker run -it -p 8888:8888 -v /$(pwd)/notebooks:/notebooks dash00/tensorflow-python3-jupyter
```

##### Use Jupyter Notebook and Tensorboard in the same time
Finally, if you want to run notebook and tensorboard at the same time, we need to have two container here with name "notebooks" and "board".
```bash
$ docker run --name notebooks -d -v /$(pwd)/notebooks:/notebooks -v /$(pwd)/logs:/logs -p 8888:8888 dash00/tensorflow-python3-jupyter /run_jupyter.sh --allow-root --NotebookApp.token=''
$
$ docker run -- board -d -v /$(pwd)/logs:/logs -p 6006:6006 dash00/tensorflow-python3-jupyter tensorboard --logdir /logs
```
Open your browser, enter `http://<container_ip>:8888` to open jupyter notebook, enter `http://<container_ip>:6006` to open tensorboard.

##### Modify and Create New Image
Let's enter current docker image, notice that the number behind root@ is your new docker image id.
```bash
$ docker run -it dash00/tensorflow-python3-jupyter /bin/bash
root@34eeac184315:/notebooks#
```
`dash00/tensorflow-python3-jupyter` mentationed that there ere python2 and python3 installed. Tensorflow was installed under python3, so the tflearn need to be installed under pyhthon3. The default python command will lead you to python2.
```
# python
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
# python3
Python 3.5.2 (default, Nov 17 2016, 17:05:23) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
```
pip install need to be done under python3 environment. For more stable version of tflearn, we need to use git to install.
```
# python3 -m pip install git+https://github.com/tflearn/tflearn.git
```
If you don't have git, please install git first.
```
# apt-get update
# apt-get install git
```
Now use pip again.
```
# python3 -m pip install git+https://github.com/tflearn/tflearn.git
# python3
Python 3.5.2 (default, Nov 17 2016, 17:05:23)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tflearn
hdf5 is not supported on this machine (please install/reinstall h5py for optimal experience)
```
Install h5py.
```
# python3 -m pip install h5py
```
Done.
```
# python3
Python 3.5.2 (default, Nov 17 2016, 17:05:23) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tflearn
>>> 
```

##### Commit, Test, and Upload
Exit the current container, using `docker commit` to submmit the copy of it.
```
# exit
$ docker commit -m="install git and tflearn" -a="nealxie3153" 34eeac184315 nealxie3153/tensorflow:latest
```
Where:
* -m: commit info.
* -a: author info.
* 34eeac184315: container id.
* nealxie3153/tensorflow:lastest: image name and version

Use `docker images` to check the new image `nealxie3153/tensorflow`.
This new image includes:
```
- git
- Jupyter Notebook
- TensorFlow
- tflearn
- scikit-learn
- pandas
- matplotlib
- numpy
- scipy
- Pillow
- python 2 and 3
```

Now let's use new image to open a container.
```bash
$ docker run  --name notebooks -d -v /$(pwd)/notebooks:/notebooks -v /$(pwd)/logs:/logs -p 8888:8888 nealxie3153/tensorflow /run_jupyter.sh --allow-root --NotebookApp.token=''
```

Finall, we can use `push` command to upload image to docker hub community.
```bash
$ docker push nealxie3153/tensorflow:latest
```

## Enjoy!

### Reference
[Get Docker CE for Ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)  
[Post-installation steps for Linux](https://docs.docker.com/engine/installation/linux/linux-postinstall/)  
[TensorFlow Offical Website](https://www.tensorflow.org/install/install_linux#InstallingDocker)  
[TensorFlow Binary Images](https://hub.docker.com/r/tensorflow/tensorflow/tags/)  
[dash00 Github](https://github.com/dash00/tensorflow-python3-jupyter)


