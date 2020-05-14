# Part 2

## First docker-compose.yml

	thomas@gentoo part2 % cat ../part1/youtube-dl/docker-compose.yml
	version: '3.5'
	
	services:
	    youtube-dl-ubuntu:
	      image: tpegeot/youtube-dl
	      build: .
	      volumes:
	        - .:/mydir
	      container_name: youtube-dl

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

	thomas@gentoo youtube-dl % mv Imgur-JY5tHqr.mp4 Imgur-JY5tHqr.mp4.old
	thomas@gentoo youtube-dl % docker-compose run youtube-dl-ubuntu https://imgur.com/JY5tHqr
	[Imgur] JY5tHqr: Downloading webpage
	[download] Destination: Imgur-JY5tHqr.mp4
	[download] 100% of 190.20KiB in 00:00

## 2.1

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
	thomas@gentoo 2.1 % cat logs.txt
	Wed, 06 May 2020 12:37:09 GMT
	Wed, 06 May 2020 12:37:12 GMT
	Wed, 06 May 2020 12:37:15 GMT
	Wed, 06 May 2020 12:37:18 GMT
	Secret message is:
	"Volume bind mount is easy"


## Whoami

	thomas@gentoo 2.1 % docker run -d -p 8000:8000 jwilder/whoami
	Unable to find image 'jwilder/whoami:latest' locally
	latest: Pulling from jwilder/whoami
	605ce1bd3f31: Pull complete
	6f87ebbce1a4: Pull complete
	42dfabe11397: Pull complete
	Digest: sha256:c621c699e1becc851a27716df9773fa9a3f6bccb331e6702330057a688fd1d5a
	Status: Downloaded newer image for jwilder/whoami:latest
	6cde6c0d6ce576746f2f10de6d24a4a3fafc51b463d0360637992b3c13e00969
	thomas@gentoo 2.1 % docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
	6cde6c0d6ce5        jwilder/whoami      "/app/http"         18 seconds ago      Up 15 seconds       0.0.0.0:8000->8000/tcp   nostalgic_elgamal
	thomas@gentoo 2.1 % docker stop  6cde6c0d6ce5
	6cde6c0d6ce5
	thomas@gentoo 2.1 % docker rm  6cde6c0d6ce5
	6cde6c0d6ce5

	thomas@gentoo whoami % docker-compose up -d
Creating network "whoami_default" with the default driver
Creating whoami_whoami_1 ... done
thomas@gentoo whoami % curl localhost:8000
I'm 3aa8bddfcc17
thomas@gentoo whoami % docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
3aa8bddfcc17        jwilder/whoami      "/app/http"         17 seconds ago      Up 14 seconds       0.0.0.0:8000->8000/tcp   whoami_whoami_1
thomas@gentoo whoami % docker stop 3aa8bddfcc17
3aa8bddfcc17

## 2.2

	thomas@gentoo 2.2 % docker-compose up -d
	Creating 22_ports_exercise_1 ... done
	thomas@gentoo 2.2 % curl http://localhost
	Ports configured correctly!!%                                                                                                                                                                



## 2.3

	thomas@gentoo 2.3 % cp -pr ../../part1/1.12/* .

	thomas@gentoo 2.3 % docker-compose up

	thomas@gentoo 2.3 % cat logs.txt
	5/5/2020, 7:46:12 PM: Connection received in root
	
## Scaling

	thomas@gentoo scaling % cp ../whoami/docker-compose.yml .
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
	^CGracefully stopping... (press Ctrl+C again to force)
	Stopping scaling_whoami_3   ... done
	Stopping scaling_whoami_2   ... done
	Stopping scaling_whoami_1   ... done
	
	
	thomas@gentoo scaling % docker-compose port --index 1 whoami 8000
	0.0.0.0:32769
	thomas@gentoo scaling % docker-compose port --index 2 whoami 8000
	0.0.0.0:32768
	thomas@gentoo scaling % docker-compose port --index 3 whoami 8000
	0.0.0.0:32770
	thomas@gentoo scaling % curl  0.0.0.0:32769
	I'm 6f0b85556e52
	thomas@gentoo scaling % curl 0.0.0.0:32768
	I'm 07a0839802f8
	thomas@gentoo scaling % curl 0.0.0.0:32770
	I'm b46b1fbc633f

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

	thomas@gentoo scaling % docker-compose up -d --scale whoami=3
	Recreating scaling_whoami_1 ... done
	Recreating scaling_whoami_2 ... done
	Recreating scaling_whoami_3 ... done
	Starting scaling_proxy_1    ... done
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 59587bf483ab
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 45cc0386f61c
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 1d4f8da41af8
	thomas@gentoo scaling % curl whoami.colasloth.com
	I'm 59587bf483ab
	thomas@gentoo scaling % docker ps
	CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                NAMES
	59587bf483ab        jwilder/whoami        "/app/http"              15 seconds ago      Up 9 seconds        8000/tcp             scaling_whoami_1
	45cc0386f61c        jwilder/whoami        "/app/http"              15 seconds ago      Up 9 seconds        8000/tcp             scaling_whoami_3
	1d4f8da41af8        jwilder/whoami        "/app/http"              15 seconds ago      Up 10 seconds       8000/tcp             scaling_whoami_2
	c0f884c29e54        jwilder/nginx-proxy   "/app/docker-entrypo…"   5 minutes ago       Up 12 seconds       0.0.0.0:80->80/tcp   scaling_proxy_1


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

thomas@gentoo scaling % docker exec -it b5d544135e5d bash
root@b5d544135e5d:/# cd /usr/share/nginx/html/

root@b5d544135e5d:/etc/nginx# cd /usr/share/nginx/html/
root@b5d544135e5d:/usr/share/nginx/html# ls -altr
total 16
-rw-r--r--  1 root root  494 Apr 14 14:19 50x.html
drwxr-xr-x  3 root root 4096 Apr 23 13:02 ..
drwxr-xr-x  2 root root 4096 Apr 23 13:02 .
-rw-rw----+ 1 1000 1000    6 May  6 13:25 index.html


thomas@gentoo scaling % ll
total 12K
-rw-rw----+ 1 thomas thomas 605  6 mai   15:26 docker-compose.yml
-rw-rw----+ 1 thomas thomas   6  6 mai   15:25 hello.html
-rw-rw----+ 1 thomas thomas   6  6 mai   15:25 world.html
thomas@gentoo scaling % chmod o+r *.html
thomas@gentoo scaling % ll
total 12K
-rw-rw----+ 1 thomas thomas 605  6 mai   15:26 docker-compose.yml
-rw-rw-r--+ 1 thomas thomas   6  6 mai   15:25 hello.html
-rw-rw-r--+ 1 thomas thomas   6  6 mai   15:25 world.html

thomas@gentoo scaling % docker exec -it b5d544135e5d bash
root@b5d544135e5d:/# cd /usr/share/nginx/html/
root@b5d544135e5d:/usr/share/nginx/html# ls -altr
total 16
-rw-r--r--  1 root root  494 Apr 14 14:19 50x.html
drwxr-xr-x  3 root root 4096 Apr 23 13:02 ..
drwxr-xr-x  2 root root 4096 Apr 23 13:02 .
-rw-rw-r--+ 1 1000 1000    6 May  6 13:25 index.html

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


thomas@gentoo scaling % vi hello.html
thomas@gentoo scaling % curl hello.colasloth.com
hello toto

## 2.4
thomas@gentoo scaling-exercise % docker-compose up
Stopping and removing scaling-exercise_compute_2 ... done
Stopping and removing scaling-exercise_compute_3 ... done
Stopping and removing scaling-exercise_compute_4 ... done
Starting calculator                              ... done
Starting load-balancer                           ... done
Starting scaling-exercise_compute_1              ... done
Attaching to calculator, load-balancer, scaling-exercise_compute_1

thomas@gentoo scaling-exercise % docker-compose up -d --scale compute=2
Starting calculator                 ... done
Starting load-balancer              ... done
Starting scaling-exercise_compute_1 ... done
Creating scaling-exercise_compute_2 ... done


## 2.5

thomas@gentoo 2.5 % docker-compose up

## Larger application
thomas@gentoo larger_application_with_volumes % docker-compose up
Creating network "larger_application_with_volumes_default" with the default driver
Pulling db (postgres:)...
latest: Pulling from library/postgres
54fec2fa59d0: Already exists
30a95add0890: Pull complete
57bc798d3c84: Pull complete
a41bedb2c5de: Pull complete
589548c3abb4: Pull complete
c4c6b75d5deb: Pull complete
8f8c045a6a99: Pull complete
69f9dd86b24d: Pull complete
45bbaba740ff: Pull complete
1761ca7befa0: Pull complete
57feb34018f4: Pull complete
bede8373accc: Pull complete
6e4c69fbe63b: Pull complete
8a7949704ab2: Pull complete
Digest: sha256:d96835c9032988c8a899cb8a3c54467dae81daaa99485de70e8c9bddd5432d92
Status: Downloaded newer image for postgres:latest
Creating db_redmine ... done
Attaching to db_redmine
db_redmine | The files belonging to this database system will be owned by user "postgres".
db_redmine | This user must also own the server process.
db_redmine |
db_redmine | The database cluster will be initialized with locale "en_US.utf8".
db_redmine | The default database encoding has accordingly been set to "UTF8".
db_redmine | The default text search configuration will be set to "english".
db_redmine |
db_redmine | Data page checksums are disabled.
db_redmine |
db_redmine | fixing permissions on existing directory /var/lib/postgresql/data ... ok
db_redmine | creating subdirectories ... ok
db_redmine | selecting dynamic shared memory implementation ... posix
db_redmine | selecting default max_connections ... 100
db_redmine | selecting default shared_buffers ... 128MB
db_redmine | selecting default time zone ... Etc/UTC
db_redmine | creating configuration files ... ok
db_redmine | running bootstrap script ... ok
db_redmine | performing post-bootstrap initialization ... ok
db_redmine | syncing data to disk ... ok
db_redmine |
db_redmine |
db_redmine | Success. You can now start the database server using:
db_redmine |
db_redmine |     pg_ctl -D /var/lib/postgresql/data -l logfile start
db_redmine |
db_redmine | initdb: warning: enabling "trust" authentication for local connections
db_redmine | You can change this by editing pg_hba.conf or using the option -A, or
db_redmine | --auth-local and --auth-host, the next time you run initdb.
db_redmine | waiting for server to start....2020-05-06 21:10:14.660 UTC [46] LOG:  starting PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_redmine | 2020-05-06 21:10:14.702 UTC [46] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_redmine | 2020-05-06 21:10:14.912 UTC [47] LOG:  database system was shut down at 2020-05-06 21:10:12 UTC
db_redmine | 2020-05-06 21:10:14.971 UTC [46] LOG:  database system is ready to accept connections
db_redmine |  done
db_redmine | server started
db_redmine |
db_redmine | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*
db_redmine |
db_redmine | waiting for server to shut down...2020-05-06 21:10:15.017 UTC [46] LOG:  received fast shutdown request
db_redmine | .2020-05-06 21:10:15.053 UTC [46] LOG:  aborting any active transactions
db_redmine | 2020-05-06 21:10:15.054 UTC [46] LOG:  background worker "logical replication launcher" (PID 53) exited with exit code 1
db_redmine | 2020-05-06 21:10:15.055 UTC [48] LOG:  shutting down
db_redmine | 2020-05-06 21:10:15.399 UTC [46] LOG:  database system is shut down
db_redmine |  done
db_redmine | server stopped
db_redmine |
db_redmine | PostgreSQL init process complete; ready for start up.
db_redmine |
db_redmine | 2020-05-06 21:10:15.471 UTC [1] LOG:  starting PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_redmine | 2020-05-06 21:10:15.471 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_redmine | 2020-05-06 21:10:15.472 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_redmine | 2020-05-06 21:10:15.563 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_redmine | 2020-05-06 21:10:15.714 UTC [55] LOG:  database system was shut down at 2020-05-06 21:10:15 UTC
db_redmine | 2020-05-06 21:10:15.765 UTC [1] LOG:  database system is ready to accept connections
^CGracefully stopping... (press Ctrl+C again to force)
Stopping db_redmine ... done
thomas@gentoo larger_application_with_volumes % ls
docker-compose.yml
thomas@gentoo larger_application_with_volumes % docker inspect db_redmine | grep -A 5 Mounts
        "Mounts": [
            {
                "Type": "volume",
                "Name": "b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162",
                "Source": "/var/lib/docker/volumes/b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162/_data",
                "Destination": "/var/lib/postgresql/data",
thomas@gentoo larger_application_with_volumes % docker volume ls
DRIVER              VOLUME NAME
local               1.8
local               14c7e5dca717f385c48e22026b39ae230ec90b59db7a24426f835984de39386e
local               4124bd11b3219bf1dfafc68c8b91d2b3483e1c4e080da4c325bfec709f7eb936
local               512848f43a607ec3b041e1b5ff9e59fea0abd6a1686cb64dc6ced8c47b99d3de
local               b2a56baf89c4b5034b9c006d44fa63e44e5a6d39dc1ff871d0d14fa11df02162
local               cae1b900e92c81354bee1fb5d15614dd9f7be79018ab1957476dcfe65f2f1113
local               logs.txt
thomas@gentoo larger_application_with_volumes % vi docker-compose.yml
thomas@gentoo larger_application_with_volumes % docker-compose down
Removing db_redmine ... done
Removing network larger_application_with_volumes_default
thomas@gentoo larger_application_with_volumes % docker-compose up
Creating network "larger_application_with_volumes_default" with the default driver
Creating volume "larger_application_with_volumes_database" with default driver
Creating db_redmine ... done
Attaching to db_redmine
db_redmine | The files belonging to this database system will be owned by user "postgres".
db_redmine | This user must also own the server process.
db_redmine |
db_redmine | The database cluster will be initialized with locale "en_US.utf8".
db_redmine | The default database encoding has accordingly been set to "UTF8".
db_redmine | The default text search configuration will be set to "english".
db_redmine |
db_redmine | Data page checksums are disabled.
db_redmine |
db_redmine | fixing permissions on existing directory /var/lib/postgresql/data ... ok
db_redmine | creating subdirectories ... ok
db_redmine | selecting dynamic shared memory implementation ... posix
db_redmine | selecting default max_connections ... 100
db_redmine | selecting default shared_buffers ... 128MB
db_redmine | selecting default time zone ... Etc/UTC
db_redmine | creating configuration files ... ok
db_redmine | running bootstrap script ... ok
db_redmine | performing post-bootstrap initialization ... ok
db_redmine | syncing data to disk ... ok
db_redmine |
db_redmine |
db_redmine | Success. You can now start the database server using:
db_redmine |
db_redmine |     pg_ctl -D /var/lib/postgresql/data -l logfile start
db_redmine |
db_redmine | initdb: warning: enabling "trust" authentication for local connections
db_redmine | You can change this by editing pg_hba.conf or using the option -A, or
db_redmine | --auth-local and --auth-host, the next time you run initdb.
db_redmine | waiting for server to start....2020-05-06 21:14:10.779 UTC [46] LOG:  starting PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_redmine | 2020-05-06 21:14:10.821 UTC [46] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_redmine | 2020-05-06 21:14:11.047 UTC [47] LOG:  database system was shut down at 2020-05-06 21:14:09 UTC
db_redmine | 2020-05-06 21:14:11.090 UTC [46] LOG:  database system is ready to accept connections
db_redmine |  done
db_redmine | server started
db_redmine |
db_redmine | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*
db_redmine |
db_redmine | waiting for server to shut down....2020-05-06 21:14:11.130 UTC [46] LOG:  received fast shutdown request
db_redmine | 2020-05-06 21:14:11.180 UTC [46] LOG:  aborting any active transactions
db_redmine | 2020-05-06 21:14:11.182 UTC [46] LOG:  background worker "logical replication launcher" (PID 53) exited with exit code 1
db_redmine | 2020-05-06 21:14:11.182 UTC [48] LOG:  shutting down
db_redmine | 2020-05-06 21:14:11.492 UTC [46] LOG:  database system is shut down
db_redmine |  done
db_redmine | server stopped
db_redmine |
db_redmine | PostgreSQL init process complete; ready for start up.
db_redmine |
db_redmine | 2020-05-06 21:14:11.592 UTC [1] LOG:  starting PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_redmine | 2020-05-06 21:14:11.592 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_redmine | 2020-05-06 21:14:11.592 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_redmine | 2020-05-06 21:14:11.700 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_redmine | 2020-05-06 21:14:11.885 UTC [55] LOG:  database system was shut down at 2020-05-06 21:14:11 UTC
db_redmine | 2020-05-06 21:14:11.927 UTC [1] LOG:  database system is ready to accept connections

^CGracefully stopping... (press Ctrl+C again to force)
Stopping db_redmine ... done
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

thomas@gentoo larger_application_with_volumes % docker-compose up
Pulling redmine (redmine:)...
latest: Pulling from library/redmine
54fec2fa59d0: Already exists
ecfc531926e1: Pull complete
6500c509d215: Pull complete
983d76fd37e5: Pull complete
2425985c1617: Pull complete
ff8fe329965c: Pull complete
3571ba549467: Pull complete
06098b83862f: Pull complete
f59ad2e53fcc: Pull complete
5c6274dfbcf1: Pull complete
2f9e8381496d: Pull complete
bf4d896ff321: Pull complete
82639d7f68fc: Pull complete
Digest: sha256:14816d9cca52d4a3a3064db582a28fae9eb434275e205451c6089dbe7a15c148
Status: Downloaded newer image for redmine:latest
Starting db_redmine ... done
Creating larger_application_with_volumes_redmine_1 ... done
Attaching to db_redmine, larger_application_with_volumes_redmine_1
db_redmine |
db_redmine | PostgreSQL Database directory appears to contain a database; Skipping initialization
db_redmine |
db_redmine | 2020-05-06 21:17:48.470 UTC [1] LOG:  starting PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_redmine | 2020-05-06 21:17:48.471 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_redmine | 2020-05-06 21:17:48.471 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_redmine | 2020-05-06 21:17:49.090 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_redmine | 2020-05-06 21:17:51.990 UTC [25] LOG:  database system was shut down at 2020-05-06 21:14:19 UTC
db_redmine | 2020-05-06 21:17:52.292 UTC [1] LOG:  database system is ready to accept connections
redmine_1  | The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x64-mingw32, x86-mswin32. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x64-mingw32 x86-mswin32`.
redmine_1  | The dependency ffi (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x64-mingw32, x86-mswin32. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x64-mingw32 x86-mswin32`.
redmine_1  | The Gemfile's dependencies are satisfied
redmine_1  | I, [2020-05-06T21:18:09.285741 #27]  INFO -- : Migrating to Setup (1)
redmine_1  | == 1 Setup: migrating =========================================================
redmine_1  | -- create_table("attachments", {:force=>true, :id=>:integer})
redmine_1  |    -> 0.0855s
redmine_1  | -- create_table("auth_sources", {:force=>true, :id=>:integer})
redmine_1  |    -> 0.0505s
redmine_1  | -- create_table("custom_fields", {:force=>true, :id=>:integer})
redmine_1  |    -> 0.0839s
redmine_1  | -- create_table("custom_fields_projects", {:id=>false, :force=>true})
redmine_1  |    -> 0.0014s
redmine_1  | -- create_table("custom_fields_trackers", {:id=>false, :force=>true})
redmine_1  |    -> 0.0012s
redmine_1  | -- create_table("custom_values", {:force=>true, :id=>:integer})
redmine_1  |    -> 0.0478s
redmine_1  | -- create_table("documents", {:force=>true, :id=>:integer})
redmine_1  |    -> 0.0505s
redmine_1  | -- add_index("documents", ["project_id"], {:name=>"documents_project_id"})
redmine_1  |    -> 0.0251s
redmine_1  | -- create_table("enumerations", {:force=>true, :id=>:integer})
redmine_1  |    -> 0.0562s
redmine_1  | -- create_table("issue_categories", {:force=>true, :id=>:integer})
...
redmine_1  |
redmine_1  | [2020-05-06 21:18:28] INFO  WEBrick 1.4.2
redmine_1  | [2020-05-06 21:18:28] INFO  ruby 2.6.6 (2020-03-31) [x86_64-linux]
redmine_1  | [2020-05-06 21:18:28] INFO  WEBrick::HTTPServer#start: pid=1 port=3000
^CGracefully stopping... (press Ctrl+C again to force)
Stopping larger_application_with_volumes_redmine_1 ... done
Stopping db_redmine                                ... done



## 2.6
thomas@gentoo part2 % mkdir 2.6
zsh: correct '2.6' to '2.5' [nyae]? n
thomas@gentoo part2 % cp -pr 2.5/* 2.6

