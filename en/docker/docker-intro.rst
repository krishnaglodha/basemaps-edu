.. module:: basemap.docker-intro
   :synopsis: Introduction to docker and it's usecase.

.. _basemap.docker-intro:

Introduction to Docker
---------------------------------------
Web development projects often times becomes complicated with the usage of several technologies, softwares used for various tasks, thus it becomes difficult over the period of time to share the projects and it's requirements in order to make it workable locally at any system. The problem arises from Operating system level all the way down to libraries and their version numbers used for the project.

Often times, VM is considered as one of the solutions which can be created for a project and shipped to wherever needed, but this strategy comes with it's own flaws. VM machines are heavy and to run such VM, it is again dependant on the user's operating system. 

**Containers** are considered as solution to this problem.

Containers has very light weight operating system of their own inside which applications can be deployed. Due to the size and speed of creating , shipping and installing containers, they are preferred over VMs. But of course there are few flaws in this as well. Example of which can be security. Since Containers are just a light weight replica of the OS, we need to be sure to install necessary packages on our own, which generally comes pre-installed while using VMs

.. figure:: img/docker.png
   :scale: 30 %
*Docker logo*

Docker is an open source engine that helps us to create images, deploy it and reuse it without worrying about what baseline OS we have on our machine. Docker images usually have their own OS which are very lightweight ( Few MegaBytes in some cases) which makes it possible to run multiple such images parallel.

advantages of using Docker
^^^^^^^^^^^^^^^^^^^^

1. Standardising Environment - Using Docker, Companies can make standard templates for the project which can be followed by Developers, Testers, etc. making sure that everyone can run the project smoothly. This saves a lot of time which goes into figuring out how to setup new machines
2. Simplicity - Dockerfile contains series of simple commands based on YAML or JSON configuration, User needs to create such files and that's all, there are no complicated steps involved
3. Quicker Deployments - Since Docker Container created layers for each commands, it can leveraging caching mechanism which helps in faster image building and shipping 

Docker Components
^^^^^^^^^^^^^^^^

Docker ecosystem consists of following components 

1. Docker Client
2. Docker Daemon
3. Docker Images
4. Docker container
5. Docker Hub

General flow of docker component is as follows 

- Docker Client is an installable piece of software which provides smooth UI to communicate with docker daemon (also known as docker server), which does all the work. Docker ships with the CLI which helps developers speed up the process of using docker via typing commands in terminal

e.g. *docker pull geosolutionsit/geoserver*

- Docker Daemon as mentioned does all the processes of pulling images from `Docker Hub <[https://hub.docker.com/](https://hub.docker.com/)>`_ , creating and maintaining containers, etc.
- Docker Images are considered as boiler plates or skeleton of the project. Each Major software has their own image hosted on `Docker Hub <[https://hub.docker.com/](https://hub.docker.com/)>`_. e.g. PostgreSQL, Python, Geoserver,etc. we generally start new project by pulling such Image and then making changes on top of it
- Docker Containers are the images that we have pulled from `Docker Hub <[https://hub.docker.com/](https://hub.docker.com/)>`_ and then we make changes on them, generally we edit them according to our own project and then host it back to `Docker Hub <[https://hub.docker.com/](https://hub.docker.com/)>`_ using our own account. Once done other developers  can pull that image and work on it .

.. figure:: img/drun.png
*Docker Workflow*

Install Docker 
^^^^^^^^^^^^^^^^

- Windows - Installation of Docker on windows depends upon version of OS you have installed. Please refer `this document  <[https://docs.docker.com/desktop/windows/install/](https://docs.docker.com/desktop/windows/install/)>`_
- Mac OS -  Depending upon which chipset of Mac are you using (Silicon or Intel) you can `download installer  <[https://docs.docker.com/desktop/mac/install/](https://docs.docker.com/desktop/windows/install/)>`_  and use it
- Linux - Depending upon linux distribution and architecture, you can `follow steps  <https://docs.docker.com/engine/install/#server)>`_   

If you are on MacOS or Windows, click on docker application to turn it on, you will see something like this in the notification bar

.. figure:: img/dockrun.png
   :scale: 30 %

**Docker Running**

Confirm Docker installation by opening Terminal of Linux/MacOS or Docker command prompt on windows and type

::

   docker -v 

and confirm if you get version 

.. figure:: img/dockterminal.png

**Docker Version confirmation in terminal**