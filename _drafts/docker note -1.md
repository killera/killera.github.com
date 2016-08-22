### docker note 1

* docker images         -- list the images locally.

* docker pull centos    -- download image to local.

* docker run -t -i ubuntu:12.04 /bin/bash 

    * `docker run` runs a container.

    * `ubuntu` is the image you would like to run.

    * `-t` flag assigns a pseudo-tty or terminal inside the new container.

    * `-i` flag allows you to make an interactive connection by grabbing the standard in (STDIN) of the container.

    * `/bin/bash` launches a Bash shell inside our container.

* docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"

    * `docker run` runs the container.
    * `-d` flag runs the container in the background (to daemonize it).


* docker ps             -- get all running containers
* docker ps -a          -- get all local containers

* docker commit -m "Added json gem" -a "Kate Smith" 0b2616b0e5a8 ouruser/sinatra:v2  -- commit image to dockerhub


* docker logs container-name                            -- check logs, `-f` if tail mode.

* docker build -t ouruser/sinatra:v2 .                  -- build docker image.

* docker tag 5db5f8471261 ouruser/sinatra:devel         -- tag image

* docker images ouruser/sinatra                         -- see all tags of image

* docker rmi training/sinatra                           -- delete image locally.

* docker rm container-name                              -- delete container
* docker stop/start/restart container-anme              -- stop/start/restart container.