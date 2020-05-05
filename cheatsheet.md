# Docker 

To list images you currently have downloaded
```
docker images
```

Build an container from an image and run it : 
```
docker run hello-world
```

Delete image from system : 
```
docker rmi hello-world
```

=> Problem:

```
Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container e6a0f2ffda58 is using its referenced image fce289e99eb9
```

To list containers that are running, run: 
thomas@gentoo part1 % docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

To list all containers : 
thomas@gentoo part1 % docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
e6a0f2ffda58        hello-world         "/hello"            3 minutes ago       Exited (0) 3 minutes ago                       distracted_mcnulty
b2b812225d78        hello-world         "/hello"            7 minutes ago       Exited (0) 7 minutes ago                       magical_dewdney
0d6abcd7df85        ubuntu              "bash"              5 days ago          Exited (0) 5 days ago                          elated_nightingale
f09236e6be7d        hello-world         "/hello"            5 days ago          Exited (0) 5 days ago                          funny_tharp
a603df77aa2e        hello-world         "/hello"            5 days ago          Exited (0) 5 days ago                          vigorous_elbakyan

Remove a single container : 
docker rm e6a0f2ffda58

Deleting all container :
docker container prune

thomas@gentoo part1 % docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
b2b812225d7817602d2001331e7e3f447461b1f554da6accc0734d22fa8e8d99
0d6abcd7df85178dcf06b14d5af7350efc9ee4a8fd94eef834ac41f59036df16
f09236e6be7d20d4fbd0fb0a8c302f91ec191e0f43b58b526be49ab24fbdc02d
a603df77aa2e726e9394b223514bfc2ca3a5ffcae0bd1d094971d22322c33fa6

Total reclaimed space: 84.27MB

thomas@gentoo part1 %  docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


thomas@gentoo part1 % docker rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:f9dfddf63636d84ef479d645ab5885156ae030f611a56f3a7ac7f2fdd86d7e4e
Deleted: sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e
Deleted: sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233688eea3c5483994a66a3

thomas@gentoo part1 % docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              4e5021d210f6        3 weeks ago         64.2MB
thomas@gentoo part1 % docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:f9dfddf63636d84ef479d645ab5885156ae030f611a56f3a7ac7f2fdd86d7e4e
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest


Run a docker container in background : 
thomas@gentoo part1 % docker run -d nginx
9e3033a931955f1162e038b7da55196787b8e21022e2d2e89769df0262b25a8d


thomas@gentoo part1 % docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
9e3033a93195        nginx               "nginx -g 'daemon of…"   24 seconds ago      Up 22 seconds       80/tcp              stupefied_chatelet

thomas@gentoo part1 % docker rm 9e3033a93195
Error response from daemon: You cannot remove a running container 9e3033a931955f1162e038b7da55196787b8e21022e2d2e89769df0262b25a8d. Stop the container before attempting removal or force remove
thomas@gentoo part1 % docker stop 9e3033a93195
9e3033a93195
thomas@gentoo part1 % docker rm 9e3033a93195  
9e3033a93195

To see all containers :
thomas@gentoo part1 %  docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                      PORTS               NAMES
294d2e0be3ab        nginx               "nginx -g 'daemon of…"   About a minute ago   Exited (0) 53 seconds ago                       elastic_johnson
448760c858b3        nginx               "nginx -g 'daemon of…"   About a minute ago   Exited (0) 47 seconds ago                       awesome_gauss
8a62dd68cb7e        nginx               "nginx -g 'daemon of…"   About a minute ago   Up About a minute           80/tcp              pedantic_pike
bf9ad06b7e80        nginx               "nginx -g 'daemon of…"   4 minutes ago        Exited (0) 4 minutes ago                        trusting_joliot

Get a tagged image : 
thomas@gentoo part1 % docker pull ubuntu:16.04 
16.04: Pulling from library/ubuntu
fe703b657a32: Pull complete 
f9df1fafd224: Pull complete 
a645a4b887f9: Pull complete 
57db7fe0b522: Pull complete 
Digest: sha256:e9938f45e51d9ff46e2b05a62e0546d0f07489b7f22fbc5288defe760599e38a
Status: Downloaded newer image for ubuntu:16.04
docker.io/library/ubuntu:16.04

Tag an image : 
thomas@gentoo part1 % docker tag ubuntu:16.04 fav_distro:xenial

thomas@gentoo part1 % docker image ls
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
ubuntu                         latest              4e5021d210f6        3 weeks ago         64.2MB
fav_distro                     xenial              77be327e4b63        7 weeks ago         124MB
ubuntu                         16.04               77be327e4b63        7 weeks ago         124MB
devopsdockeruh/pull_exercise   latest              d9854bc0e13a        13 months ago       75.3MB

thomas@gentoo part1 % docker run fav_distro:xenial uptime
 11:59:19 up 46 min,  0 users,  load average: 0.25, 0.14, 0.17
thomas@gentoo part1 % uptime
 13:59:28 up 47 min,  3 users,  load average: 0,23, 0,14, 0,17

 thomas@gentoo part1 % docker run -d --name looper ubuntu:16.04 sh -c 'while true; do date; sleep 1; done'
abd131b1abcbaf5fab1cff31221d0f211cac6ad5187e77d987e4d43cda4fa11e
thomas@gentoo part1 % docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
abd131b1abcb        ubuntu:16.04        "sh -c 'while true; …"   7 seconds ago       Up 5 seconds                            looper
thomas@gentoo part1 % docker logs -f looper
Tue Apr 14 12:00:16 UTC 2020
Tue Apr 14 12:00:17 UTC 2020
Tue Apr 14 12:00:18 UTC 2020

Let’s follow ‘-f’ the output of logs : 
docker logs -f looper

$ docker start looper 

$ docker attach --sig-proxy=false looper 
  Mon Jan 15 19:27:54 UTC 2018 
  Mon Jan 15 19:27:55 UTC 2018 
  ^C 
  
  => The container will continue running. Control+c now only disconnects you from the STDOUT.

Enter container interactively : :
thomas@gentoo part1 % docker exec -it looper bash 
root@abd131b1abcb:/# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.0   4500  1572 ?        Ss   12:02   0:00 sh -c while true; do date; sleep 1; done
root       264  2.4  0.0  18240  3348 pts/0    Ss   12:04   0:00 bash
root       284  0.0  0.0   4372   808 ?        S    12:05   0:00 sleep 1
root       285  0.0  0.0  34420  2948 pts/0    R+   12:05   0:00 ps aux

Create dockerfile : 
thomas@gentoo docker_project % cat Dockerfile 
FROM ubuntu:16.04 

WORKDIR /mydir 
RUN touch hello.txt 
COPY local.txt . 
RUN apt-get update && apt-get install -y wget
RUN wget http://example.com/index.html 
CMD ["/bin/bash"]

Create image :
thomas@gentoo docker_project % docker build -t myfirst .

thomas@gentoo docker_project % docker run -it myfirst
root@39d655c48731:/mydir# ls
hello.txt  index.html  local.txt
root@39d655c48731:/mydir# touch manually.txt
root@39d655c48731:/mydir# exit

Diff between container and image : 
thomas@gentoo docker_project % docker diff 39d655c48731
C /mydir
A /mydir/manually.txt
C /root
A /root/.bash_history


Build image from container : 
thomas@gentoo docker_project % docker commit 39d655c48731 myfirst-pluschanges 

sha256:3d80cb4966b800ab2b375981c454994b543222e88267269f0df4ebcab5bf41ea

thomas@gentoo docker_project % docker image ls
REPOSITORY                          TAG                 IMAGE ID            CREATED              SIZE
myfirst-pluschanges                 latest              3d80cb4966b8        About a minute ago   157MB
myfirst                             latest              24e6b050c9c7        4 minutes ago        157MB
<none>                              <none>              04f99a2d4edd        5 minutes ago        157MB
ubuntu                              latest              4e5021d210f6        3 weeks ago          64.2MB
fav_distro                          xenial              77be327e4b63        7 weeks ago          124MB
ubuntu                              16.04               77be327e4b63        7 weeks ago          124MB
devopsdockeruh/pull_exercise        latest              d9854bc0e13a        13 months ago        75.3MB
devopsdockeruh/exec_bash_exercise   latest              489b6d8f2ab8        14 months ago        897MB
appropriate/curl                    latest              d37e1f717dc0        2 years ago          5.5MB
thomas@gentoo docker_project % docker run myfirst-pluschanges ls -l 
total 4
-rw-r--r-- 1 root root    0 Apr 14 12:28 hello.txt
-rw-r--r-- 1 root root 1256 Oct 17 07:18 index.html
-rw-rw---- 1 root root    0 Apr 14 12:29 local.txt
-rw-r--r-- 1 root root    0 Apr 14 12:32 manually.txt

