[![CircleCI](https://circleci.com/gh/Rubusch/docker__sandbox.svg?style=shield)](https://circleci.com/gh/Rubusch/docker__sandbox)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.html)


# Sandbox

Base container for all kind of projects.  


## Tools Needed

```
$ sudo apt-get install -y libffi-dev libssl-dev
$ sudo apt-get install -y python3-dev
$ sudo apt-get install -y python3 python3-pip
$ pip3 install docker-compose
```
Make sure, ``~/.local`` is within ``$PATH`` or re-link e.g. it to ``/usr/local``.
```
$ sudo usermod -aG docker $USER
```


## Build

```
$ cd ./docker
$ docker-compose up
```


## Usage

```
$ docker-compose -f ./docker-compose.yml run --rm sandbox /bin/bash
docker$ build.sh
```

Make sure the device is plugged (/dev/ttyACM0 exists)  

NB: Appending ``--privileged`` is not _safe_! Mainly this is used for such things as connecting the USB (SEGGER) the easiest way possible.  

NB: Append ``/bin/bash`` to enter the current container for debugging  


## Cleanup

Remove orphaned containers  
```
$ cd ./docker
$ docker-compose up -d --remove-orphans
```

Remove dangling container images, etc.  
```
$ docker system prune -f
```

Remove an docker image  
```
$ docker images
    REPOSITORY               TAG        IMAGE ID       CREATED         SIZE
    user/ubu1804             20211028   8b0855782faf   11 months ago   2.99GB
$ docker rmi -f 8b0855782faf
```

Check via docker-compose (also offers possibility to remove an image)  
```
$ docker-compose ps
```

For more consult the specific help and manpages.  
