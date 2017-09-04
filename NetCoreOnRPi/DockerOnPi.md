# Preparing the Raspberry PI for using Docker containers
The Raspberry PI can run containers as announced  [here](https://www.raspberrypi.org/blog/docker-comes-to-raspberry-pi/).
Discussions on the advantages of running containers on a small device is outside the scope of this document.

## Install the docker client
The first step is installing the docker CLI on the Pi:
```
curl -sSL https://get.docker.com | sh
```
At the end of the installation, you can add your user to the 'docker' group to avoid using `sudo` to issue commands on the CLI. In this case you will need  to log off and back on.
This can be done with the following command:
```
sudo usermod -aG docker pi
```

## Test the installation running a container
You can now run containers on the platform, but ensure picking only containers compatible with the ARMv7 architecture, otherwise you will receive an error.
A community driven effort has been done to make popular container available on different platform. The home project can be found here: [https://hub.docker.com/u/arm32v7/](https://hub.docker.com/u/arm32v7/).
As [you can see from the docker hub](https://hub.docker.com/u/arm32v7/), there  are many containers you can run on the PI.
The simplest possible container is the "hello world". Running this command
```
docker run --rm arm32v7/hello-world
```
you should see this output:
```
Unable to find image 'arm32v7/hello-world:latest' locally
latest: Pulling from arm32v7/hello-world
a13a95db9748: Pull complete
Digest: sha256:2692c9c633fec185c503d13008ab6b769ed1a72b976af201013ba0128acc5722
Status: Downloaded newer image for arm32v7/hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```
The first line says `Unable to find image 'arm32v7/hello-world:latest' locally`. This just means that the specified image is not available locally and will be downloaded from the docker hub.
The rest of the message, specifically `Hello from Docker!` means that everything works correctly.

Another more sophisticated image you can test is [Busybox](https://busybox.net/about.html), a collection of popular Unix utilities, made available under the GPLv2 license.
```
docker run -it --rm arm32v7/busybox
```
The "-it" option allows the interactivity with the container from the console/keyboard.
It will give you a prompt where you can experiment the busybox commands inside the container.
Once finished just type 'exit':
```
$ docker run -it --rm arm32v7/busybox
/ # exit
$
```

## Preparing a container using NetCore 2.0
[Follow this link to prepare a container, publishing to the docker hub and finally deploy it to the Raspberry PI](DockerViaHub.md).