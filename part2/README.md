# Part 2

## First docker-compose.yml

Create container from an image hosted on docker hub : 

	thomas@gentoo part2 % cat ../part1/youtube-dl/docker-compose.yml
	version: '3.5'
		services:
	    youtube-dl-ubuntu:
	      image: tpegeot/youtube-dl
	      build: .
	      volumes:
	        - .:/mydir
	      container_name: youtube-dl

Build image :

	thomas@gentoo youtube-dl % docker-compose build
	Building youtube-dl-ubuntu
	Step 1/7 : FROM ubuntu:16.04
	 ---> 77be327e4b63
	Step 2/7 : WORKDIR /mydir
	 ---> Using cache
	 ---> 16f2ddda0a82
	Step 3/7 : RUN apt-get update && apt-get install -y curl python
	 ---> Using cache
	 ---> 24ae205db850
	Step 4/7 : RUN curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
	 ---> Using cache
	 ---> 96008870882e
	Step 5/7 : RUN chmod a+x /usr/local/bin/youtube-dl
	 ---> Using cache
	 ---> b51a352becbb
	Step 6/7 : ENV LC_ALL=C.UTF-8
	 ---> Using cache
	 ---> 5cd2dc25cd57
	Step 7/7 : ENTRYPOINT ["/usr/local/bin/youtube-dl"]
	 ---> Using cache
	 ---> eb1edb9e3d8b
	Successfully built eb1edb9e3d8b
	Successfully tagged tpegeot/youtube-dl:latest

Push image to docker hub : 

	thomas@gentoo youtube-dl % docker-compose push
	Pushing youtube-dl-ubuntu (tpegeot/youtube-dl:latest)...
	The push refers to repository [docker.io/tpegeot/youtube-dl]
	aa2aabd950de: Layer already exists
	8bdcb8ae1a63: Layer already exists
	1a166ea50532: Layer already exists
	3084003fc9f0: Pushed
	4ae3adcb66cb: Layer already exists
	aa6685385151: Layer already exists
	0040d8f00d7e: Layer already exists
	9e6f810a2aab: Pushed
	latest: digest: sha256:ea40be19b6207b4115bd572213ee36c44ee2a138dd9f4a958e9876d0267bb381 size: 1991

Clean old file : 

	thomas@gentoo youtube-dl % mv Imgur-JY5tHqr.mp4 Imgur-JY5tHqr.mp4.old

Run container : 

	thomas@gentoo youtube-dl % docker-compose run youtube-dl-ubuntu https://imgur.com/JY5tHqr
	[Imgur] JY5tHqr: Downloading webpage
	[download] Destination: Imgur-JY5tHqr.mp4
	[download] 100% of 190.20KiB in 00:00

## 2.1

Run devopsdockeruh/first_volume_exercise with docker-compose : 

	thomas@gentoo 2.1 % docker-compose up
	Creating network "21_default" with the default driver
	Creating 21_app_1 ... done
	Attaching to 21_app_1
	app_1  | (node:1) ExperimentalWarning: The fs.promises API is experimental
	app_1  | Wrote to file /usr/app/logs.txt
	app_1  | Wrote to file /usr/app/logs.txt
	app_1  | Wrote to file /usr/app/logs.txt
	app_1  | Wrote to file /usr/app/logs.txt
	app_1  | Wrote to file /usr/app/logs.txt
	^CGracefully stopping... (press Ctrl+C again to force)
	Stopping 21_app_1 ... done
	thomas@gentoo 2.1 % ls
	docker-compose.yml  logs.txt

Check log content : 

	thomas@gentoo 2.1 % cat logs.txt
	Wed, 06 May 2020 12:37:09 GMT
	Wed, 06 May 2020 12:37:12 GMT
	Wed, 06 May 2020 12:37:15 GMT
	Wed, 06 May 2020 12:37:18 GMT
	Secret message is:
	"Volume bind mount is easy"


## Whoami

Run container from jwilder/whoami : 

	thomas@gentoo 2.1 % docker run -d -p 8000:8000 jwilder/whoami
	Unable to find image 'jwilder/whoami:latest' locally
	latest: Pulling from jwilder/whoami
	605ce1bd3f31: Pull complete
	6f87ebbce1a4: Pull complete
	42dfabe11397: Pull complete
	Digest: sha256:c621c699e1becc851a27716df9773fa9a3f6bccb331e6702330057a688fd1d5a
	Status: Downloaded newer image for jwilder/whoami:latest
	6cde6c0d6ce576746f2f10de6d24a4a3fafc51b463d0360637992b3c13e00969

Check container ID : 

	thomas@gentoo 2.1 % docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
	6cde6c0d6ce5        jwilder/whoami      "/app/http"         18 seconds ago      Up 15 seconds       0.0.0.0:8000->8000/tcp   nostalgic_elgamal

Stop container : 

	thomas@gentoo 2.1 % docker stop  6cde6c0d6ce5
	6cde6c0d6ce5

Delete container : 

	thomas@gentoo 2.1 % docker rm  6cde6c0d6ce5
	6cde6c0d6ce5

Start new container with docker-compose : 

	thomas@gentoo whoami % docker-compose up -d
	Creating network "whoami_default" with the default driver
	Creating whoami_whoami_1 ... done

Access web page : 

	thomas@gentoo whoami % curl localhost:8000
	I'm 3aa8bddfcc17


Get container ID : 

	thomas@gentoo whoami % docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
	3aa8bddfcc17        jwilder/whoami      "/app/http"         17 seconds ago      Up 14 seconds       0.0.0.0:8000->8000/tcp   whoami_whoami_1

Stop container : 

	thomas@gentoo whoami % docker stop 3aa8bddfcc17
	3aa8bddfcc17

## 2.2

Run devopsdockeruh/ports_exercise : 

	thomas@gentoo 2.2 % docker-compose up -d
	Creating 22_ports_exercise_1 ... done

Check if container works : 

	thomas@gentoo 2.2 % curl http://localhost
	Ports configured correctly!!%                                                                


## 2.3

Get frontend and backend files : 

	thomas@gentoo 2.3 % cp -pr ../../part1/1.12/* .

Start frontend and backend with docker-compose : 

	thomas@gentoo 2.3 % docker-compose up

Check logs: 

	thomas@gentoo 2.3 % cat logs.txt
	5/5/2020, 7:46:12 PM: Connection received in root
	
## Scaling

Get file from previous exercise : 

	thomas@gentoo scaling % cp ../whoami/docker-compose.yml .

Start three instances :

	thomas@gentoo scaling % docker-compose up --scale whoami=3
	Creating network "scaling_default" with the default driver
	WARNING: The "whoami" service specifies a port on the host. If multiple containers for this service are created on a single host, the port will clash.
	Creating scaling_whoami_1 ...
	Creating scaling_whoami_2 ...
	Creating scaling_whoami_1 ... error
	WARNING: Host is already in use by another container
	WARNING: Host is already in use by another container
	ERROR: for scaling_whoami_2  Cannot start service whoami: driver failed programming external connectivity on endpoint scaling_whoami_2 (bcaf74cc2aa180f06cb13433e05eb32eabf5c0850cfa006571bbdff4255d2cc0): Bind for 0.0.0.0:8000 failed: port is already allocated
	ERROR: for scaling_whoami_1  Cannot start service whoami: driver failed programming external connectivity on endpoint scaling_whoami_1 (12745a65b6e382508e405d171f1b3be166dc28fe4e40f7eeed0bfbacd6dce6f6): Bind for 0.0.0.0:8000 failed: port is already allocated
	ERROR: for whoami  Cannot start service whoami: driver failed programming external connectivity on endpoint scaling_whoami_2 (bcaf74cc2aa180f06cb13433e05eb32eabf5c0850cfa006571bbdff4255d2cc0): Bind for 0.0.0.0:8000 failed: port is already allocated
	ERROR: Encountered errors while bringing up the project.

After modifing configuration, start the instances again : 

	thomas@gentoo scaling % docker-compose up --scale whoami=3
	Recreating scaling_whoami_1 ... done
	Recreating scaling_whoami_2 ... done
	Recreating scaling_whoami_3 ... done
	Attaching to scaling_whoami_2, scaling_whoami_1, scaling_whoami_3
	whoami_1  | Listening on :8000
	whoami_2  | Listening on :8000
	whoami_3  | Listening on :8000
	whoami_1  | I'm 6f0b85556e52
	whoami_2  | I'm 07a0839802f8
	whoami_3  | I'm b46b1fbc633f

Identify listening ports : 
	
	thomas@gentoo scaling % docker-compose port --index 1 whoami 8000
	0.0.0.0:32769
	thomas@gentoo scaling % docker-compose port --index 2 whoami 8000
	0.0.0.0:32768
	thomas@gentoo scaling % docker-compose port --index 3 whoami 8000
	0.0.0.0:32770

Use Curl to get container ID : 

	thomas@gentoo scaling % curl  0.0.0.0:32769
	I'm 6f0b85556e52
	thomas@gentoo scaling % curl 0.0.0.0:32768
	I'm 07a0839802f8
	thomas@gentoo scaling % curl 0.0.0.0:32770
	I'm b46b1fbc633f

Try with proxy : 

	thomas@gentoo scaling % docker-compose up -d --scale whoami=3
	Starting scaling_whoami_1 ... done
	Starting scaling_whoami_2 ... done
	Starting scaling_whoami_3 ... done
	Starting scaling_proxy_1  ... done
	thomas@gentoo scaling % curl localhost:80
	<html>
	<head><title>503 Service Temporarily Unavailable</title></head>
	<body>
	<center><h1>503 Service Temporarily Unavailable</h1></center>
	<hr><center>nginx/1.17.6</center>
	</body>
	</html>

After reconfiguing VIRTUAL_HOST and VIRTUAL_PORT, try again : 

	thomas@gentoo scaling % docker-compose up -d --scale whoami=3
	Recreating scaling_whoami_1 ... done
	Recreating scaling_whoami_2 ... done
	Recreating scaling_whoami_3 ... done
	Starting scaling_proxy_1    ... done

Try proxy again : 

	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 59587bf483ab
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 45cc0386f61c
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 1d4f8da41af8
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 59587bf483ab

Get container ID : 

	thomas@gentoo scaling % docker ps
	CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                NAMES
	59587bf483ab        jwilder/whoami        "/app/http"              15 seconds ago      Up 9 seconds        8000/tcp             scaling_whoami_1
	45cc0386f61c        jwilder/whoami        "/app/http"              15 seconds ago      Up 9 seconds        8000/tcp             scaling_whoami_3
	1d4f8da41af8        jwilder/whoami        "/app/http"              15 seconds ago      Up 10 seconds       8000/tcp             scaling_whoami_2
	c0f884c29e54        jwilder/nginx-proxy   "/app/docker-entrypo…"   5 minutes ago       Up 12 seconds       0.0.0.0:80->80/tcp   scaling_proxy_1

But world.colasloth.com and hello.colasloth.com are failing : 

	thomas@gentoo scaling % curl world.colasloth.com
	<html>
	<head><title>403 Forbidden</title></head>
	<body>
	<center><h1>403 Forbidden</h1></center>
	<hr><center>nginx/1.17.10</center>
	</body>
	</html>

	thomas@gentoo scaling % curl hello.colasloth.com
	<html>
	<head><title>403 Forbidden</title></head>
	<body>
	<center><h1>403 Forbidden</h1></center>
	<hr><center>nginx/1.17.10</center>
	</body>
	</html>

Check file permissions in container : 

	thomas@gentoo scaling % docker exec -it b5d544135e5d bash
	root@b5d544135e5d:/# cd /usr/share/nginx/html/
	root@b5d544135e5d:/etc/nginx# cd /usr/share/nginx/html/
	root@b5d544135e5d:/usr/share/nginx/html# ls -altr
	total 16
	-rw-r--r--  1 root root  494 Apr 14 14:19 50x.html
	drwxr-xr-x  3 root root 4096 Apr 23 13:02 ..
	drwxr-xr-x  2 root root 4096 Apr 23 13:02 .
	-rw-rw----+ 1 1000 1000    6 May  6 13:25 index.html

Check file permissions on the host : 

	thomas@gentoo scaling % ll
	total 12K
	-rw-rw----+ 1 thomas thomas 605  6 mai   15:26 docker-compose.yml
	-rw-rw----+ 1 thomas thomas   6  6 mai   15:25 hello.html
	-rw-rw----+ 1 thomas thomas   6  6 mai   15:25 world.html

Fix incorrect permissions : 

	thomas@gentoo scaling % chmod o+r *.html
	thomas@gentoo scaling % ll
	total 12K
	-rw-rw----+ 1 thomas thomas 605  6 mai   15:26 docker-compose.yml
	-rw-rw-r--+ 1 thomas thomas   6  6 mai   15:25 hello.html
	-rw-rw-r--+ 1 thomas thomas   6  6 mai   15:25 world.html

Check permissions : 

	thomas@gentoo scaling % docker exec -it b5d544135e5d bash
	root@b5d544135e5d:/# cd /usr/share/nginx/html/
	root@b5d544135e5d:/usr/share/nginx/html# ls -altr
	total 16
	-rw-r--r--  1 root root  494 Apr 14 14:19 50x.html
	drwxr-xr-x  3 root root 4096 Apr 23 13:02 ..
	drwxr-xr-x  2 root root 4096 Apr 23 13:02 .
	-rw-rw-r--+ 1 1000 1000    6 May  6 13:25 index.html

New try : 

	thomas@gentoo scaling % curl hello.colasloth.com
	hello
	thomas@gentoo scaling % curl world.colasloth.com
	world
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 45cc0386f61c
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 1d4f8da41af8
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 59587bf483ab
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 45cc0386f61c


Modify html file localy : 

	thomas@gentoo scaling % vi hello.html

Check if file is really modified : 

	thomas@gentoo scaling % curl hello.colasloth.com
	hello toto

## 2.4

Run docker-hy/scaling-exercise containers : 

	thomas@gentoo scaling-exercise % docker-compose up
	Stopping and removing scaling-exercise_compute_2 ... done
	Stopping and removing scaling-exercise_compute_3 ... done
	Stopping and removing scaling-exercise_compute_4 ... done
	Starting calculator                              ... done
	Starting load-balancer                           ... done
	Starting scaling-exercise_compute_1              ... done
	Attaching to calculator, load-balancer, scaling-exercise_compute_1

=> fail!

Scale up to two compute node : 

	thomas@gentoo scaling-exercise % docker-compose up -d --scale compute=2
	Starting calculator                 ... done
	Starting load-balancer              ... done
	Starting scaling-exercise_compute_1 ... done
	Creating scaling-exercise_compute_2 ... done

=> success


![2.4](https://github.com/tpegeot/devopswithdocker/raw/master/part2/2.4/Screenshot_20200506_223353.png "2.4")


## 2.5

Add redis  :

	thomas@gentoo part2 % diff 2.3/docker-compose.yml 2.5/docker-compose.yml 
	17a18,24
	>       environment:
	>         REDIS: redis
	>     redis:
	>       image: redis
	>       volumes:
	>         - ./redis_data:/data
	>       entrypoint: ["redis-server","--appendonly","yes"]


## Larger application

Run larger application : 

	thomas@gentoo larger_application_with_volumes % docker-compose up
	Creating network "larger_application_with_volumes_default" with the default driver
	...

Inspect container : 

	thomas@gentoo larger_application_with_volumes % docker inspect db_redmine | grep -A 5 Mounts
	        "Mounts": [
	            {
	                "Type": "volume",
	                "Name": "b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162",
	                "Source": "/var/lib/docker/volumes/b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162/_data",
	                "Destination": "/var/lib/postgresql/data",

Get volume list : 

	thomas@gentoo larger_application_with_volumes % docker volume ls
	DRIVER              VOLUME NAME
	local               1.8
	local               14c7e5dca717f385c48e22026b39ae230ec90b59db7a24426f835984de39386e
	local               4124bd11b3219bf1dfafc68c8b91d2b3483e1c4e080da4c325bfec709f7eb936
	local               512848f43a607ec3b041e1b5ff9e59fea0abd6a1686cb64dc6ced8c47b99d3de
	local               b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162
	local               cae1b900e92c81354bee1fb5d15614dd9f7be79018ab1957476dcfe65f2f1113
	local               logs.txt

Stop application : 

	thomas@gentoo larger_application_with_volumes % docker-compose down
	....

Create separate volum, run containers again and check volumes : 

	thomas@gentoo larger_application_with_volumes % docker volume ls
	DRIVER              VOLUME NAME
	local               1.8
	local               14c7e5dca717f385c48e22026b39ae230ec90b59db7a24426f835984de39386e
	local               4124bd11b3219bf1dfafc68c8b91d2b3483e1c4e080da4c325bfec709f7eb936
	local               512848f43a607ec3b041e1b5ff9e59fea0abd6a1686cb64dc6ced8c47b99d3de
	local               b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162
	local               cae1b900e92c81354bee1fb5d15614dd9f7be79018ab1957476dcfe65f2f1113
	local               larger_application_with_volumes_database
	local               logs.txt


	thomas@gentoo larger_application_with_volumes % docker inspect db_redmine | grep -A 5 Mounts
	        "Mounts": [
	            {
	                "Type": "volume",
	                "Name": "larger_application_with_volumes_database",
	                "Source": "/var/lib/docker/volumes/larger_application_with_volumes_database/_data",
	                "Destination": "/var/lib/postgresql/data",

Add redmine application : 

	thomas@gentoo larger_application_with_volumes % docker-compose up
	Pulling redmine (redmine:)...
	latest: Pulling from library/redmine



## 2.6
:
Get files from previous exercise : 

	thomas@gentoo part2 % cp -pr 2.5/* 2.6

Add db : 

	thomas@gentoo part2 % diff 2.5/docker-compose.yml 2.6/docker-compose.yml -u
	--- 2.5/docker-compose.yml      2020-05-06 23:00:35.575623960 +0200
	+++ 2.6/docker-compose.yml      2020-05-07 08:44:48.193460249 +0200
	@@ -17,8 +17,24 @@
	         - ./logs.txt:/mydir/logs.txt
	       environment:
	         REDIS: redis
	+        DB_USERNAME: postgres
	+        DB_PASSWORD: example
	+        DB_NAME: postgres
	+        DB_HOST: db
	     redis:
	       image: redis
	       volumes:
	         - ./redis_data:/data
	       entrypoint: ["redis-server","--appendonly","yes"]
	+    db:
	+      image: postgres
	+      restart: unless-stopped
	+      environment:
	+        POSTGRES_PASSWORD: example
	+      container_name: db
	+      volumes:
	+        - backend_db:/var/lib/postgresql/data
	+
	+volumes:
	+  backend_db:
	+


After starting the containers, check application : 

## 2.7

## 2.8

Modify docker-compose.yml to remove port definition and add nginx service 

	thomas@gentoo part2 % diff 2.6/docker-compose.yml 2.8/docker-compose.yml
	8,9c8,9
	<       ports: 
	<         - 5000:5000
	---
	>       depends_on: 
	>         - backend
	14,15d13
	<       ports: 
	<         - 8000:8000
	23a22,24
	>       depends_on: 
	>         - redis
	>         - db
	36a38,46
	>     reverse:
	>       image: nginx
	>       volumes:
	>         - ./nginx.conf:/etc/nginx/nginx.conf
	>       ports: 
	>         - 80:80
	>       depends_on: 
	>         - frontend
	>         - backend

Nginx configuration : 

	thomas@gentoo part2 % cat 2.8/nginx.conf 
	  events { worker_connections 1024; }
	  http {
	    server {
	      listen 80;
	      location / {
	        proxy_pass http://frontend:5000;
	      }
	      location /api/ {
	        proxy_pass http://backend:8000/;
	      }
	    }
	  }

/ is really important afer backend:8000! 

Screenshot : 

![2.8](https://github.com/tpegeot/devopswithdocker/raw/master/part2/2.8/Screenshot_20200514_115454.png "2.8")

## 2.9

After delete db container, create database directory and copy postgresql file into this new directory : 

	thomas@gentoo devopswithdocker % sudo cp -r /var/lib/docker/volumes/29_backend_db/_data/* database

Modify docker-compose.yml : 


	thomas@gentoo part2 % diff 2.8/docker-compose.yml 2.9/docker-compose.yml
	7a8,9
	>       environment:
	>         API_URL: http://localhost/api
	21a24
	>         FRONT_URL: 'http://localhost'
	37c40
	<         - backend_db:/var/lib/postgresql/data
	---
	>         - ./database:/var/lib/postgresql/data
	47,49d49
	< 
	< volumes:
	<   backend_db:

=> Modifications from 2.10 are already include here!

## 2.10

I had to set API_URL and FRONT_URL in docker-compose.yml : 

	thomas@gentoo devopswithdocker % diff part2/2.8/docker-compose.yml part2/2.10/docker-compose.yml -u
	--- part2/2.8/docker-compose.yml        2020-05-14 11:40:05.924773407 +0200
	+++ part2/2.10/docker-compose.yml       2020-05-14 13:29:57.161067508 +0200
	@@ -5,6 +5,8 @@
	       build: 
	         context: .
	         dockerfile: Dockerfile-frontend
	+      environment:
	+        API_URL: http://localhost/api
	       depends_on: 
	         - backend
	     backend:
	@@ -19,6 +21,7 @@
	         DB_PASSWORD: example
	         DB_NAME: postgres
	         DB_HOST: db
	+        FRONT_URL: 'http://localhost'
	       depends_on: 
	         - redis
	         - db

Screenshot : 

![2.10](https://github.com/tpegeot/devopswithdocker/raw/master/part2/2.10/Screenshot_20200514_142740.png "2.10")