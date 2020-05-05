# Exercices - Part 1

## 1.1 Getting started

Start 3 containers from image that does not automatically exit, such as nginx, detached.

	thomas@gentoo part1 % docker run -d nginx
	8a62dd68cb7e1711a063ee6706620731af165d897ccb5db426b3f7b8aa4b02cc
	thomas@gentoo part1 % docker run -d nginx
	448760c858b37cdf76053a755d923559907c4a85195f8c2288638edb0189805c
	thomas@gentoo part1 % docker run -d nginx
	294d2e0be3abcda5695ffc00e9b506229b1d9c2ca218c9d931ef85d7c182d9a7
	
	thomas@gentoo part1 % docker container ls
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
	294d2e0be3ab        nginx               "nginx -g 'daemon of…"   9 seconds ago       Up 6 seconds        80/tcp              elastic_johnson
	448760c858b3        nginx               "nginx -g 'daemon of…"   12 seconds ago      Up 9 seconds        80/tcp              awesome_gauss
	8a62dd68cb7e        nginx               "nginx -g 'daemon of…"   14 seconds ago      Up 12 seconds       80/tcp              pedantic_pike

Stop 2 of the containers leaving 1 up

	thomas@gentoo part1 % docker stop 294d2e0be3ab
	294d2e0be3ab
	thomas@gentoo part1 % docker stop 448760c858b3
	448760c858b3
	
	thomas@gentoo part1 % docker container ls     
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
	8a62dd68cb7e        nginx               "nginx -g 'daemon of…"   38 seconds ago      Up 36 seconds       80/tcp              pedantic_pike
	
	thomas@gentoo part1 %  docker ps -a
	CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                      PORTS               NAMES
	294d2e0be3ab        nginx               "nginx -g 'daemon of…"   About a minute ago   Exited (0) 53 seconds ago                       elastic_johnson
	448760c858b3        nginx               "nginx -g 'daemon of…"   About a minute ago   Exited (0) 47 seconds ago                       awesome_gauss
	8a62dd68cb7e        nginx               "nginx -g 'daemon of…"   About a minute ago   Up About a minute           80/tcp              pedantic_pike
	bf9ad06b7e80        nginx               "nginx -g 'daemon of…"   4 minutes ago        Exited (0) 4 minutes ago                        trusting_joliot

Note : bf9ad06b7e80 was started earlier as a test

## 1.2 Cleanup
All containers are stopped : 
	thomas@gentoo part1 % docker container ls -a    
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
	294d2e0be3ab        nginx               "nginx -g 'daemon of…"   4 minutes ago       Exited (0) 3 minutes ago                        elastic_johnson
	448760c858b3        nginx               "nginx -g 'daemon of…"   4 minutes ago       Exited (0) 3 minutes ago                        awesome_gauss
	8a62dd68cb7e        nginx               "nginx -g 'daemon of…"   4 minutes ago       Exited (0) 14 seconds ago                       pedantic_pike
	bf9ad06b7e80        nginx               "nginx -g 'daemon of…"   7 minutes ago       Exited (0) 7 minutes ago                        trusting_joliot
	
Cleaning containers: 
	thomas@gentoo part1 % docker rm 294d2e0be3ab 448760c858b3 8a62dd68cb7e bf9ad06b7e80
	294d2e0be3ab
	448760c858b3
	8a62dd68cb7e
	bf9ad06b7e80

All the containers got deleted
	thomas@gentoo part1 % docker container ls -a                                       
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

Deleting all images : 
	thomas@gentoo part1 % docker image rm nginx ubuntu hello-world
	Untagged: nginx:latest
	Untagged: nginx@sha256:282530fcb7cd19f3848c7b611043f82ae4be3781cb00105a1d593d7e6286b596
	Deleted: sha256:ed21b7a8aee9cc677df6d7f38a641fa0e3c05f65592c592c9f28c42b3dd89291
	Deleted: sha256:8a305f371a6c3c445a1dfc500c1364743868a269ab8cdaf95902692e82168352
	Deleted: sha256:d079ef06ec1f10a8050887365f9a940b39547ba6bcc46b16a463e740984f3223
	Deleted: sha256:c3a984abe8a88059915bb6c7a1d249fd1ccc16d931334ac8816540b0eb686b45
	Untagged: ubuntu:latest
	Untagged: ubuntu@sha256:bec5a2727be7fff3d308193cfde3491f8fba1a2ba392b7546b43a051853a341d
	Deleted: sha256:4e5021d210f65ebe915670c7089120120bc0a303b90208592851708c1b8c04bd
	Deleted: sha256:1d9112746e9d86157c23e426ce87cc2d7bced0ba2ec8ddbdfbcc3093e0769472
	Deleted: sha256:efcf4a93c18b5d01aa8e10a2e3b7e2b2eef0378336456d8653e2d123d6232c1e
	Deleted: sha256:1e1aa31289fdca521c403edd6b37317bf0a349a941c7f19b6d9d311f59347502
	Deleted: sha256:c8be1b8f4d60d99c281fc2db75e0f56df42a83ad2f0b091621ce19357e19d853
	Untagged: hello-world:latest
	Untagged: hello-world@sha256:f9dfddf63636d84ef479d645ab5885156ae030f611a56f3a7ac7f2fdd86d7e4e
	Deleted: sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e
	Deleted: sha256:af0b15c8625bb1938f1d7b17081031f649fd14e6b233688eea3c5483994a66a3

All containers and images got deleted : 
	thomas@gentoo part1 % docker image ls                         
	REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
	
	thomas@gentoo part1 % docker ps -a
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

## 1.3 Hello Docker Hub
Start pull exercise image : 
	thomas@gentoo part1 % docker run -it devopsdockeruh/pull_exercise
	Unable to find image 'devopsdockeruh/pull_exercise:latest' locally
	latest: Pulling from devopsdockeruh/pull_exercise
	8e402f1a9c57: Pull complete 
	5e2195587d10: Pull complete 
	6f595b2fc66d: Pull complete 
	165f32bf4e94: Pull complete 
	67c4f504c224: Pull complete 
	Digest: sha256:7c0635934049afb9ca0481fb6a58b16100f990a0d62c8665b9cfb5c9ada8a99f
	Status: Downloaded newer image for devopsdockeruh/pull_exercise:latest
	Give me the password: basics
	You found the correct password. Secret message is:
	"This is the secret message"

Secret message is : "This is the secret message"

## 1.4
Start devopsdockeruh/exec_bash_exercise image : 
	thomas@gentoo part1 % docker run -d  -it devopsdockeruh/exec_bash_exercise                                           
	Unable to find image 'devopsdockeruh/exec_bash_exercise:latest' locally
	latest: Pulling from devopsdockeruh/exec_bash_exercise
	741437d97401: Pull complete 
	34d8874714d7: Pull complete 
	0a108aa26679: Pull complete 
	7f0334c36886: Pull complete 
	65c95cb8b3be: Pull complete 
	a36b708560f8: Pull complete 
	4090f912e6c7: Pull complete 
	ce5fe2607c2e: Pull complete 
	9400f5f657d6: Pull complete 
	c4919883f7fa: Pull complete 
	Digest: sha256:c463832132d1fb0b8b3b60348a6fc36fda7512a4ef2d1050e8bea7b6a6d7a2f3
	Status: Downloaded newer image for devopsdockeruh/exec_bash_exercise:latest
	ab950ad53dbff6e22e87bc777cd48917901e27c435362c0d0b73003e1a394417

Get container ID  : 
	thomas@gentoo part1 % docker container ls                                
	CONTAINER ID        IMAGE                               COMMAND             CREATED             STATUS              PORTS               NAMES
	2501146fbd56        devopsdockeruh/exec_bash_exercise   "node index"        12 seconds ago      Up 10 seconds                           flamboyant_wiles

Access container : 
	thomas@gentoo part1 % docker exec -it  2501146fbd56 bash
	root@2501146fbd56:/usr/app# tail -f ./logs.txt
	Tue, 14 Apr 2020 12:11:54 GMT
	Secret message is:
	"Docker is easy"
	Tue, 14 Apr 2020 12:12:00 GMT
	Tue, 14 Apr 2020 12:12:03 GMT
	Tue, 14 Apr 2020 12:12:06 GMT
	Tue, 14 Apr 2020 12:12:09 GMT
	Secret message is:
	"Docker is easy"
	Tue, 14 Apr 2020 12:12:15 GMT
	Tue, 14 Apr 2020 12:12:18 GMT
	Tue, 14 Apr 2020 12:12:21 GMT
	Tue, 14 Apr 2020 12:12:24 GMT

## 1.5
Start Ubuntu image
	thomas@gentoo part1 % docker run -d -it ubuntu
	c3f659f60dbb42810a53d8c59ad8e1df031003f48bdbf8168669a02e5332d34f

Get container id : 
	thomas@gentoo part1 % docker container ls              
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
	c3f659f60dbb        ubuntu              "/bin/bash"         7 seconds ago       Up 5 seconds                            charming_dijkstra

Access container and run bash
	thomas@gentoo part1 % docker exec -it c3f659f60dbb bash
	root@c3f659f60dbb:/# apt-get install curl
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	E: Unable to locate package curl
	root@c3f659f60dbb:/# apt-get update      
	...
	root@c3f659f60dbb:/# apt-get install curl

Run application : 
	thomas@gentoo part1 % docker exec -it c3f659f60dbb sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
	Input website:
	helsinki.fi
	Searching..
	<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
	<html><head>
	<title>301 Moved Permanently</title>
	</head><body>
	<h1>Moved Permanently</h1>
	<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
	</body></html>

"Smarter" solution 1 : 
	thomas@gentoo part1 % docker run -it ubuntu sh -c 'apt-get update;apt-get install curl;echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'   
	Get:1 http://archive.ubuntu.com/ubuntu bionic InRelease [242 kB]
	Get:2 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]      
	Get:3 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]       
	Get:4 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]                   
	Get:5 http://archive.ubuntu.com/ubuntu bionic/main amd64 Packages [1344 kB]                    
	Get:6 http://archive.ubuntu.com/ubuntu bionic/restricted amd64 Packages [13.5 kB]              
	Get:7 http://archive.ubuntu.com/ubuntu bionic/multiverse amd64 Packages [186 kB]     
	Get:8 http://archive.ubuntu.com/ubuntu bionic/universe amd64 Packages [11.3 MB]
	Get:9 http://security.ubuntu.com/ubuntu bionic-security/restricted amd64 Packages [44.6 kB]
	Get:10 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages [889 kB]
	Get:11 http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages [839 kB]
	Get:12 http://security.ubuntu.com/ubuntu bionic-security/multiverse amd64 Packages [8213 B]
	Get:13 http://archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [1372 kB]
	Get:14 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [1183 kB]
	Get:15 http://archive.ubuntu.com/ubuntu bionic-updates/restricted amd64 Packages [59.0 kB]
	Get:16 http://archive.ubuntu.com/ubuntu bionic-updates/multiverse amd64 Packages [12.6 kB]
	Get:17 http://archive.ubuntu.com/ubuntu bionic-backports/main amd64 Packages [2496 B]
	Get:18 http://archive.ubuntu.com/ubuntu bionic-backports/universe amd64 Packages [4247 B]
	Fetched 17.8 MB in 6s (3127 kB/s)                         
	Reading package lists... Done
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	The following additional packages will be installed:
	  ca-certificates krb5-locales libasn1-8-heimdal libcurl4 libgssapi-krb5-2 libgssapi3-heimdal libhcrypto4-heimdal
	  libheimbase1-heimdal libheimntlm0-heimdal libhx509-5-heimdal libk5crypto3 libkeyutils1 libkrb5-26-heimdal libkrb5-3
	  libkrb5support0 libldap-2.4-2 libldap-common libnghttp2-14 libpsl5 libroken18-heimdal librtmp1 libsasl2-2 libsasl2-modules
	  libsasl2-modules-db libsqlite3-0 libssl1.1 libwind0-heimdal openssl publicsuffix
	Suggested packages:
	  krb5-doc krb5-user libsasl2-modules-gssapi-mit | libsasl2-modules-gssapi-heimdal libsasl2-modules-ldap libsasl2-modules-otp
	  libsasl2-modules-sql
	The following NEW packages will be installed:
	  ca-certificates curl krb5-locales libasn1-8-heimdal libcurl4 libgssapi-krb5-2 libgssapi3-heimdal libhcrypto4-heimdal
	  libheimbase1-heimdal libheimntlm0-heimdal libhx509-5-heimdal libk5crypto3 libkeyutils1 libkrb5-26-heimdal libkrb5-3
	  libkrb5support0 libldap-2.4-2 libldap-common libnghttp2-14 libpsl5 libroken18-heimdal librtmp1 libsasl2-2 libsasl2-modules
	  libsasl2-modules-db libsqlite3-0 libssl1.1 libwind0-heimdal openssl publicsuffix
	0 upgraded, 30 newly installed, 0 to remove and 12 not upgraded.
	Need to get 4835 kB of archives.
	After this operation, 14.8 MB of additional disk space will be used.
	Do you want to continue? [Y/n] y
	Get:1 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libssl1.1 amd64 1.1.1-1ubuntu2.1~18.04.5 [1300 kB]
	Get:2 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 openssl amd64 1.1.1-1ubuntu2.1~18.04.5 [613 kB]
	Get:3 http://archive.ubuntu.com/ubuntu bionic/main amd64 ca-certificates all 20180409 [151 kB]
	Get:4 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libsqlite3-0 amd64 3.22.0-1ubuntu0.3 [498 kB]
	Get:5 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 krb5-locales all 1.16-2ubuntu0.1 [13.5 kB]
	Get:6 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libkrb5support0 amd64 1.16-2ubuntu0.1 [30.9 kB]
	Get:7 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libk5crypto3 amd64 1.16-2ubuntu0.1 [85.6 kB]
	Get:8 http://archive.ubuntu.com/ubuntu bionic/main amd64 libkeyutils1 amd64 1.5.9-9.2ubuntu2 [8720 B]
	Fetched 4835 kB in 2s (3215 kB/s)          
	...
	debconf: delaying package configuration, since apt-utils is not installed
	Selecting previously unselected package libssl1.1:amd64.
	(Reading database ... 4046 files and directories currently installed.)
	...
	done.
	Input website:
	helsinki.fi
	Searching..
	<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
	<html><head>
	<title>301 Moved Permanently</title>
	</head><body>
	<h1>Moved Permanently</h1>
	<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
	</body></html>

"Smarter" solution 2 : 
Search for a curl image : 
thomas@gentoo part1 % docker search curl
	NAME                             DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
	appropriate/curl                 Alpine-based image with just curl               56                                      [OK]
	tutum/curl                       Base ubuntu image with curl preinstalled        23                                      [OK]
	curlimages/curl                  official docker image for curl - command lin…   16                                      
	governmentpaas/curl-ssl          curl-ssl                                        8                                       [OK]
	lucashalbert/curl                A lightweight multiarch alpine linux contain…   4                                       
	pstauffer/curl                   Small docker image with curl based on alpine…   3                                       [OK]
	mwendler/curl                    a minimal curl setup                            2                                       [OK]
	gempesaw/curl-jq                 appropriate/curl (alpine) plus jq               2                                       [OK]
	paasmule/curl-ssl-jq                                                             2                                       
	jessecollier/curl-alpine         A quick container for running curl commands     1                                       [OK]
	hectormolinero/curl              A statically linked build of cURL in a Docke…   1                                       
	theyorkshiredev/curl-container   A small alpine linux container with curl ins…   1                                       [OK]
	pcfkubo/curl                     Curl on Alpine                                  0                                       
	luky/curl                        curl + update grep                              0                                       
	maiwj/curl                       cURL supports by Alpine in a docker containe…   0                                       [OK]
	solidnerd/curl                   curl                                            0                                       [OK]
	ibmcom/curl                      Docker Image for IBM Cloud Private-CE (Commu…   0                                       
	paasmule/curl-ssl-git                                                            0                                       
	dosowisko/curl                   Alpine image with just curl (including SSL c…   0                                       
	deardooley/curl                  Simple cURL utility based on Alpine             0                                       [OK]
	plenus/curl                      Docker image with curl                          0                                       [OK]
	uzyexe/curl                      curl is an open source command line tool and…   0                                       [OK]
	everpeace/curl-jq                ubuntu-slim based curl + jq box                 0                                       [OK]
	discoenv/curl-wrapper            Container that wraps the curl_wrapper.pl scr…   0                                       
	robotgraves/curl-git             Alpine with cURL and GIT                        0                                       [OK]

Run application : 
	thomas@gentoo part1 % docker run -it appropriate/curl sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
	Unable to find image 'appropriate/curl:latest' locally
	latest: Pulling from appropriate/curl
	ff3a5c916c92: Pull complete 
	3151abf94102: Pull complete 
	58ae3cb4aac4: Pull complete 
	Digest: sha256:c8bf5bbec6397465a247c2bb3e589bb77e4f62ff88a027175ecb2d9e4f12c9d7
	Status: Downloaded newer image for appropriate/curl:latest
	Input website:
	helsinki.fi
	Searching..
	<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
	<html><head>
	<title>301 Moved Permanently</title>
	</head><body>
	<h1>Moved Permanently</h1>
	<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
	</body></html>

## 1.6
First docker file : 
	thomas@gentoo 1.6 % cat 1/Dockerfile 
	FROM devopsdockeruh/overwrite_cmd_exercise

Second docker file : 
	thomas@gentoo 1.6 % cat 2/Dockerfile 
	FROM devopsdockeruh/overwrite_cmd_exercise

	CMD ["-c","0"]

Run docker clock : 
	thomas@gentoo 2 % docker run docker-clock       
	1
	2

## 1.7
Docker file :
	thomas@gentoo 1.7 % cat Dockerfile 
	FROM ubuntu:16.04 
	
	WORKDIR /mydir 
	COPY curl.sh . 
	RUN apt-get update && apt-get install -y curl
	RUN chmod +x curl.sh
	CMD ["/mydir/curl.sh"]

Build image : 
	thomas@gentoo 1.7 % docker build -t curler .
	Sending build context to Docker daemon  3.072kB
	Step 1/6 : FROM ubuntu:16.04
	 ---> 77be327e4b63
	Step 2/6 : WORKDIR /mydir
	 ---> Using cache
	 ---> 16f2ddda0a82
	Step 3/6 : COPY curl.sh .
	 ---> Using cache
	 ---> 6fcb214dbeba
	Step 4/6 : RUN apt-get update && apt-get install -y curl
	 ---> Using cache
	 ---> 4888eff625d4
	Step 5/6 : RUN chmod +x curl.sh
	 ---> Using cache
	 ---> 2a871172c424
	Step 6/6 : CMD ["/mydir/curl.sh"]
	 ---> Running in ba04f65ddfe1
	Removing intermediate container ba04f65ddfe1
	 ---> a48e5f275626
	Successfully built a48e5f275626
	Successfully tagged curler:latest

Run container : 
	thomas@gentoo 1.7 % docker run -i curler    
	Input website:
	helsinki.fi
	Searching..
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100   231  100   231    0     0    976      0 --:--:-- --:--:-- --:--:--   974
	<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
	<html><head>
	<title>301 Moved Permanently</title>
	</head><body>
	<h1>Moved Permanently</h1>
	<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
	</body></html>

## 1.8
Start the container with bind mount so that the logs are created into your filesystem : 
	thomas@gentoo 1.8 % touch $(pwd)/logs.txt
	thomas@gentoo 1.8 % docker run -v $(pwd)/logs.txt:/usr/app/logs.txt -i devopsdockeruh/first_volume_exercise

## 1.9
	thomas@gentoo part1 % docker run -p 80:80 devopsdockeruh/ports_exercise                                      
	Unable to find image 'devopsdockeruh/ports_exercise:latest' locally
	latest: Pulling from devopsdockeruh/ports_exercise
	cd784148e348: Pull complete 
	9abca35fefbf: Pull complete 
	7fc670963d22: Pull complete 
	893040f9bc16: Pull complete 
	b0ae6401e570: Pull complete 
	Digest: sha256:2ff93dd0c744aee7a8f00bc9558d09fd6279493da0d01769fe600f78fb4593f2
	Status: Downloaded newer image for devopsdockeruh/ports_exercise:latest
	
	> ports_exercise@1.0.0 start /usr/app
	> node index.js
	
	Listening on port 80, this means inside of the container. Use -p to map the port to a port of your local machine.

## 1.10
Get code from github : 
	thomas@gentoo 1.10 % git clone https://github.com/docker-hy/frontend-example-docker
	Clonage dans 'frontend-example-docker'...
	remote: Enumerating objects: 8, done.
	remote: Counting objects: 100% (8/8), done.
	remote: Compressing objects: 100% (8/8), done.
	remote: Total 141 (delta 2), reused 1 (delta 0), pack-reused 133
	Réception d'objets: 100% (141/141), 259.36 Kio | 948.00 Kio/s, fait.
	Résolution des deltas: 100% (60/60), fait.

Note : i chose to download the code outside of the container in order to have a versioned image

Build image
	thomas@gentoo 1.10 % docker build -t frontend-example-docker .
	Sending build context to Docker daemon  1.817MB
	Step 1/11 : FROM ubuntu:16.04
	 ---> 77be327e4b63
	Step 2/11 : WORKDIR /mydir
	 ---> Using cache
	 ---> 16f2ddda0a82
	Step 3/11 : COPY frontend-example-docker/ .
	 ---> 83ac0745d21a
	Step 4/11 : RUN apt-get update && apt-get install -y curl
	 ---> Running in 0db1962a8776
	...
	Step 5/11 : RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
	 ---> Running in d7df76e6f06d
	
	## Installing the NodeSource Node.js 10.x repo...
	
	
	## Populating apt-get cache...
	
	+ apt-get update
	Hit:1 http://archive.ubuntu.com/ubuntu xenial InRelease
	Hit:2 http://archive.ubuntu.com/ubuntu xenial-updates InRelease
	Hit:3 http://archive.ubuntu.com/ubuntu xenial-backports InRelease
	Hit:4 http://security.ubuntu.com/ubuntu xenial-security InRelease
	Reading package lists...
	
	## Installing packages required for setup: apt-transport-https lsb-release...
	
	+ apt-get install -y apt-transport-https lsb-release > /dev/null 2>&1
	
	
	## Confirming "xenial" is supported...
	
	+ curl -sLf -o /dev/null 'https://deb.nodesource.com/node_10.x/dists/xenial/Release'
	
	## Adding the NodeSource signing key to your keyring...
	
	+ curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -
	OK
	
	## Creating apt sources list file for the NodeSource Node.js 10.x repo...
	
	+ echo 'deb https://deb.nodesource.com/node_10.x xenial main' > /etc/apt/sources.list.d/nodesource.list
	+ echo 'deb-src https://deb.nodesource.com/node_10.x xenial main' >> /etc/apt/sources.list.d/nodesource.list
	
	## Running `apt-get update` for you...
	
	+ apt-get update
	Hit:1 http://archive.ubuntu.com/ubuntu xenial InRelease
	Hit:2 http://archive.ubuntu.com/ubuntu xenial-updates InRelease
	Hit:3 http://archive.ubuntu.com/ubuntu xenial-backports InRelease
	Hit:4 http://security.ubuntu.com/ubuntu xenial-security InRelease
	Get:5 https://deb.nodesource.com/node_10.x xenial InRelease [4584 B]
	Get:6 https://deb.nodesource.com/node_10.x xenial/main amd64 Packages [767 B]
	Fetched 5351 B in 0s (7326 B/s)
	Reading package lists...
	
	## Run `sudo apt-get install -y nodejs` to install Node.js 10.x and npm
	## You may also need development tools to build native addons:
	     sudo apt-get install gcc g++ make
	## To install the Yarn package manager, run:
	     curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
	     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
	     sudo apt-get update && sudo apt-get install yarn
	
	
	Removing intermediate container d7df76e6f06d
	 ---> 88a725566e81
	Step 6/11 : RUN apt install -y nodejs
	 ---> Running in dcc5112ec0a5
	
	WARNING: apt does not have a stable CLI interface. Use with caution in scripts.
	
	Reading package lists...^[[5~
	Building dependency tree...
	Reading state information...
	The following additional packages will be installed:
	  libpython-stdlib libpython2.7-minimal libpython2.7-stdlib python
	  python-minimal python2.7 python2.7-minimal
	Suggested packages:
	  python-doc python-tk python2.7-doc binutils binfmt-support
	The following NEW packages will be installed:
	  libpython-stdlib libpython2.7-minimal libpython2.7-stdlib nodejs python
	  python-minimal python2.7 python2.7-minimal
	0 upgraded, 8 newly installed, 0 to remove and 2 not upgraded.
	...
	Removing intermediate container dcc5112ec0a5
	 ---> 889f95f31643
	
	Step 7/11 : RUN node -v
	 ---> Running in 64915ff14419
	v10.20.1
	Removing intermediate container 64915ff14419
	 ---> bda68af69a07
	Step 8/11 : RUN npm -v
	 ---> Running in 689d371d8069
	6.14.4
	Removing intermediate container 689d371d8069
	 ---> 1e1d1df47b38
	Step 9/11 : RUN npm install
	 ---> Running in 5b97b42a011e
	
	> core-js@2.6.11 postinstall /mydir/node_modules/core-js
	> node -e "try{require('./postinstall')}catch(e){}"
	
	Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!
	
	The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
	> https://opencollective.com/core-js
	> https://www.patreon.com/zloirock
	
	Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)
	
	npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.12 (node_modules/fsevents):
	npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.12: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
	
	added 1462 packages from 472 contributors and audited 12930 packages in 19.76s
	
	28 packages are looking for funding
	  run `npm fund` for details
	
	found 1 low severity vulnerability
	  run `npm audit fix` to fix them, or `npm audit` for details
	Removing intermediate container 5b97b42a011e
	 ---> 98b57d951e3c
	Step 10/11 : EXPOSE 5000
	 ---> Running in 63c200aff587
	Removing intermediate container 63c200aff587
	 ---> 0fd57f5f7b47
	Step 11/11 : CMD ["/usr/bin/npm","start"]
	 ---> Running in f99a2ef1ced2
	Removing intermediate container f99a2ef1ced2
	 ---> ebc42b92510a
	Successfully built ebc42b92510a
	Successfully tagged frontend-example-docker:latest

Run container : 
	thomas@gentoo 1.10 % docker run -it -p 5000:5000 frontend-example-docker
	
	> frontend-example-docker@1.0.0 start /mydir
	> webpack --mode production && serve -s -l 5000 dist
	
	
	Hash: 9a4d23964a0815fedb61
	Version: webpack 4.42.1
	Time: 15086ms
	Built at: 05/04/2020 7:18:26 PM
	                                 Asset       Size  Chunks                    Chunk Names
	0ab54153eeeca0ce03978cc463b257f7.woff2   39.2 KiB          [emitted]
	  13db00b7a34fee4d819ab7f9838cc428.eot   96.3 KiB          [emitted]
	  701ae6abd4719e9c2ada3535a497b341.eot   30.4 KiB          [emitted]
	  82f60bd0b94a1ed68b1e6e309ce2e8c3.svg    105 KiB          [emitted]
	  8e3c7f5520f5ae906c6cf6d7f3ddcd19.eot    104 KiB          [emitted]
	  962a1bf31c081691065fe333d9fa8105.svg    382 KiB          [emitted]  [big]
	  9c74e172f87984c48ddf5c8108cabe67.png   27.5 KiB          [emitted]
	 a046592bac8f2fd96e994733faf3858c.woff   62.2 KiB          [emitted]
	  a1a749e89f578a49306ec2b055c073da.svg    496 KiB          [emitted]  [big]
	  a3e2211dddcba197b5bbf2aa9d5d9a9a.svg   3.19 KiB          [emitted]
	  ad97afd3337e8cda302d10ff5a4026b8.ttf   30.2 KiB          [emitted]
	  b87b9ba532ace76ae9f6edfe9f72ded2.ttf    103 KiB          [emitted]
	  bff6c47a9da5c7cfa2e8a552e2df3a78.svg    3.2 KiB          [emitted]
	  c5ebe0b32dc1b5cc449a76c4204d13bb.ttf   96.1 KiB          [emitted]
	cd6c777f1945164224dee082abaea03a.woff2     12 KiB          [emitted]
	e8c322de9658cbeb8a774b6624167c2c.woff2   53.2 KiB          [emitted]
	 ef60a4f6c25ef7f39f2d25a748dbecfe.woff   14.4 KiB          [emitted]
	 faff92145777a3cbaf8e7367b4807987.woff   49.3 KiB          [emitted]
	                            index.html  454 bytes          [emitted]
	                              main.css  127 bytes       0  [emitted]         main
	                               main.js   21.8 KiB       0  [emitted]         main
	                    vendors~main-1.css    602 KiB       1  [emitted]  [big]  vendors~main
	                       vendors~main.js    342 KiB       1  [emitted]  [big]  vendors~main
	           vendors~main.js.LICENSE.txt   1.37 KiB          [emitted]
	Entrypoint main [big] = vendors~main-1.css vendors~main.js main.css main.js
	  [7] ./node_modules/semantic-ui-react/dist/es/lib/index.js + 1 modules 2.94 KiB {1} [built]
	      |    2 modules
	 [51] ./node_modules/semantic-ui-react/dist/es/elements/Icon/Icon.js + 1 modules 6.22 KiB {1} [built]
	      |    2 modules
	 [80] ./node_modules/react-redux/es/index.js + 19 modules 37 KiB {1} [built]
	      |    20 modules
	 [93] ./node_modules/semantic-ui-react/dist/es/elements/Label/Label.js + 2 modules 10.6 KiB {1} [built]
	      |    3 modules
	[212] (webpack)/buildin/global.js 472 bytes {1} [built]
	[251] ./src/assets/toscalogo_color.svg 82 bytes {0} [built]
	[252] ./src/assets/toscalogo_grayscale.svg 82 bytes {0} [built]
	[270] multi @babel/polyfill ./src 40 bytes {0} [built]
	[464] (webpack)/buildin/harmony-module.js 573 bytes {1} [built]
	[466] ./src/assets/custom.css 39 bytes {0} [built]
	[602] ./src/index.js + 18 modules 42.1 KiB {0} [built]
	      | ./src/index.js 609 bytes [built]
	      | ./src/util/store.js 481 bytes [built]
	      | ./util/common.js 117 bytes [built]
	      | ./src/util/apiConnection.js 4.57 KiB [built]
	      | ./src/util/redux/index.js 219 bytes [built]
	      | ./src/util/redux/messageReducer.js 2.15 KiB [built]
	      | ./src/util/redux/simpleReducer.js 1.86 KiB [built]
	      | ./src/util/common.js 221 bytes [built]
	      |     + 11 hidden modules
	[603] ./node_modules/semantic-ui-react/dist/es/elements/Button/Button.js + 3 modules 17.7 KiB {1} [built]
	      |    4 modules
	[612] ./node_modules/react-router-dom/es/BrowserRouter.js + 12 modules 41 KiB {1} [built]
	      |    13 modules
	[614] ./node_modules/react-router-dom/es/Switch.js + 1 modules 3.35 KiB {1} [built]
	      |    2 modules
	[615] ./node_modules/react-router-dom/es/Route.js + 1 modules 5.9 KiB {1} [built]
	      |    2 modules
	    + 989 hidden modules
	
	WARNING in asset size limit: The following asset(s) exceed the recommended size limit (244 KiB).
	This can impact web performance.
	Assets:
	  962a1bf31c081691065fe333d9fa8105.svg (382 KiB)
	  a1a749e89f578a49306ec2b055c073da.svg (496 KiB)
	  vendors~main-1.css (602 KiB)
	  vendors~main.js (342 KiB)
	
	WARNING in entrypoint size limit: The following entrypoint(s) combined asset size exceeds the recommended limit (244 KiB). This can impact web performance.
	Entrypoints:
	  main (966 KiB)
	      vendors~main-1.css
	      vendors~main.js
	      main.css
	      main.js
	
	WARNING in webpack performance recommendations:
	You can limit the size of your bundles by using import() or require.ensure to lazy load some parts of your application.
	For more info visit https://webpack.js.org/guides/code-splitting/
	Child html-webpack-plugin for "index.html":
	     1 asset
	    Entrypoint undefined = index.html
	    [2] (webpack)/buildin/global.js 472 bytes {0} [built]
	    [3] (webpack)/buildin/module.js 497 bytes {0} [built]
	        + 2 hidden modules
	Child mini-css-extract-plugin node_modules/css-loader/index.js!node_modules/semantic-ui-css/semantic.min.css:
	    Entrypoint mini-css-extract-plugin = *
	       19 modules
	Child mini-css-extract-plugin node_modules/css-loader/index.js!src/assets/custom.css:
	    Entrypoint mini-css-extract-plugin = *
	    [0] ./node_modules/css-loader!./src/assets/custom.css 340 bytes {0} [built]
	        + 1 hidden module
	ERROR: Cannot copy to clipboard: write EPIPE
	
	   ┌────────────────────────────────────────────────┐
	   │                                                │
	   │   Serving!                                     │
	   │                                                │
	   │   - Local:            http://localhost:5000    │
	   │   - On Your Network:  http://172.17.0.3:5000   │
	   │                                                │
	   └────────────────────────────────────────────────┘

## 1.12
Download code from github : 
	thomas@gentoo 1.11 % git clone https://github.com/docker-hy/backend-example-docker
	Clonage dans 'backend-example-docker'...
	remote: Enumerating objects: 7, done.
	remote: Counting objects: 100% (7/7), done.
	remote: Compressing objects: 100% (6/6), done.
	remote: Total 91 (delta 1), reused 4 (delta 1), pack-reused 84
	Réception d'objets: 100% (91/91), 115.73 Kio | 669.00 Kio/s, fait.
	Résolution des deltas: 100% (33/33), fait.

Build image : 
	thomas@gentoo 1.11 % docker build -t backend-example-docker .
	Sending build context to Docker daemon  372.2kB
	Step 1/11 : FROM ubuntu:16.04
	 ---> 77be327e4b63
	Step 2/11 : WORKDIR /mydir
	 ---> Using cache
	 ---> 16f2ddda0a82
	Step 3/11 : COPY backend-example-docker/ .
	 ---> ce090a698312
	Step 4/11 : RUN apt-get update && apt-get install -y curl
	 ---> Running in 28023126dd61
	..
	done.
	Removing intermediate container 28023126dd61
	 ---> 697a706d1451
	Step 5/11 : RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
	 ---> Running in 2c4220c854d6
	
	## Installing the NodeSource Node.js 10.x repo...
	
	
	## Populating apt-get cache...
	
	+ apt-get update
	Hit:1 http://archive.ubuntu.com/ubuntu xenial InRelease
	Hit:2 http://archive.ubuntu.com/ubuntu xenial-updates InRelease
	Hit:3 http://archive.ubuntu.com/ubuntu xenial-backports InRelease
	Hit:4 http://security.ubuntu.com/ubuntu xenial-security InRelease
	Reading package lists...
	
	## Installing packages required for setup: apt-transport-https lsb-release...
	
	+ apt-get install -y apt-transport-https lsb-release > /dev/null 2>&1
	
	## Confirming "xenial" is supported...
	
	+ curl -sLf -o /dev/null 'https://deb.nodesource.com/node_10.x/dists/xenial/Release'
	
	## Adding the NodeSource signing key to your keyring...
	
	+ curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -
	OK
	
	## Creating apt sources list file for the NodeSource Node.js 10.x repo...
	
	+ echo 'deb https://deb.nodesource.com/node_10.x xenial main' > /etc/apt/sources.list.d/nodesource.list
	+ echo 'deb-src https://deb.nodesource.com/node_10.x xenial main' >> /etc/apt/sources.list.d/nodesource.list
	
	## Running `apt-get update` for you...
	
	+ apt-get update
	Hit:1 http://security.ubuntu.com/ubuntu xenial-security InRelease
	Hit:2 http://archive.ubuntu.com/ubuntu xenial InRelease
	Hit:3 http://archive.ubuntu.com/ubuntu xenial-updates InRelease
	Hit:4 http://archive.ubuntu.com/ubuntu xenial-backports InRelease
	Get:5 https://deb.nodesource.com/node_10.x xenial InRelease [4584 B]
	Get:6 https://deb.nodesource.com/node_10.x xenial/main amd64 Packages [767 B]
	Fetched 5351 B in 0s (11.5 kB/s)
	Reading package lists...
	
	## Run `sudo apt-get install -y nodejs` to install Node.js 10.x and npm
	## You may also need development tools to build native addons:
	     sudo apt-get install gcc g++ make
	## To install the Yarn package manager, run:
	     curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
	     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
	     sudo apt-get update && sudo apt-get install yarn
	
	
	Removing intermediate container 2c4220c854d6
	 ---> 0e242631b46c
	Step 6/11 : RUN apt install -y nodejs
	 ---> Running in 97d3d8778729
	...
	Removing intermediate container 97d3d8778729
	 ---> 7ec9bca177f7
	Step 7/11 : RUN node -v
	 ---> Running in 4d62653b2143
	v10.20.1
	Removing intermediate container 4d62653b2143
	 ---> 9e11d3f83718
	Step 8/11 : RUN npm -v
	 ---> Running in dc660ebc1348
	6.14.4
	Removing intermediate container dc660ebc1348
	 ---> 41415cb5ed50
	Step 9/11 : RUN npm install
	 ---> Running in 0694a734637f
	
	> nodemon@1.18.9 postinstall /mydir/node_modules/nodemon
	> node bin/postinstall || exit 0
	
	[32mLove nodemon? You can now support the project via the open collective:[22m[39m
	 > [96m[1mhttps://opencollective.com/nodemon/donate[0m
	
	[91mnpm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.12 (node_modules/fsevents):
	npm WARN notsup[0m[91m SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.12: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
	[0m[91m
	[0madded 482 packages from 283 contributors and audited 4739 packages in 7.352s
	found 0 vulnerabilities
	
	Removing intermediate container 0694a734637f
	 ---> fbc2a7a9960a
	Step 10/11 : EXPOSE 8000
	 ---> Running in 8f1edb2e8eab
	Removing intermediate container 8f1edb2e8eab
	 ---> 99c7db2414f8
	Step 11/11 : CMD ["/usr/bin/npm","start"]
	 ---> Running in 232ef1e96fec
	Removing intermediate container 232ef1e96fec
	 ---> 92a9f9a9a875
	Successfully built 92a9f9a9a875
	Successfully tagged backend-example-docker:latest

Start container:
	thomas@gentoo 1.11 % docker run -it -p 8000:8000 -v $(pwd)/logs.txt:/mydir/logs.txt backend-example-docker
	
	> backend-example-docker@1.0.0 start /mydir
	> cross-env NODE_ENV=production node index.js

	Browserslist: caniuse-lite is outdated. Please run next command `npm update caniuse-lite browserslist`
	Started on port 8000

Check log file after one http request : 
	thomas@gentoo ~ % cat tmp/devopswithdocker/part1/1.11/logs.txt
	5/4/2020, 7:43:40 PM: Connection received in root

Get container ID
	thomas@gentoo ~ % docker ps
	CONTAINER ID        IMAGE                               COMMAND                CREATED             STATUS              PORTS                    NAMES
	9e89725b2894        backend-example-docker              "/usr/bin/npm start"   2 minutes ago       Up 2 minutes        0.0.0.0:8000->8000/tcp   xenodochial_shamir

Stop container :
	thomas@gentoo ~ % docker stop 9e89725b2894
	9e89725b2894

Start container : 
	thomas@gentoo ~ % docker start 9e89725b2894
	9e89725b2894

Check log after second http requests : 
	thomas@gentoo ~ % cat tmp/devopswithdocker/part1/1.11/logs.txt
	5/4/2020, 7:43:40 PM: Connection received in root
	5/4/2020, 7:45:18 PM: Connection received in root


