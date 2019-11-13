This is an **unofficial** ARM 32 bit docker image for Freenet.

The binaries used are from the official Freenet releases. You can verify that
they are trustworthy by checking the sha256 of each and comparing it to the official
release files.

It should run on Raspberry Pi and other ARM devices. It was tested on an Odroid-XU4.

This image is an ARM port of the Freenet docker image from [5nafu](https://github.com/5nafu/docker-freenet).

Usage
=====

Clone this repository to your docker host:

    git clone https://github.com/nuess0r/docker-freenet.git

Then build the image with docker:

    docker build

Then edit the docker-compose.yml file to set the port and volume configuration
the way you want it, then run it:

    docker-compose -f docker-compose.yml up -d

Alternatively you can run the image directly:

    docker run --name freenet -v $HOME/freenet/data:/data -v $HOME/freenet/config:/conf -p 127.0.0.1:8888:8888 -p 127.0.0.1:9481:9481 -p 12345:12345/udp -p 12346:12346/udp

Afterward Freenet should be running, and you should be able to access fproxy on port
8888, either on the local machine or by connecting to it through an ssh tunnel:

    ssh -L 8888:127.0.0.1:8888 -N user@dockerhost

By default Freenet is only accessible from localhost. This can be overwritten in
the `freenet.ini`, via the environment variable `allowedhosts` or in the docker-compose file:

    docker run --env allowedhosts=192.168.0.0/24 ...

Freenet opens two UDP Ports for communication with the outside world. By default this image uses 12345 & 123456, which can be overwritten by setting
the environment variables `darknetport` and `opennetport` to the desired port number (see above and in the `docker-compose.yml`).


Details
=====

This image ships a set of Freenet binaries inside the image and properly separates
them from user data (they are stored in docker volumes), so that the image can
be torn down and updated without risk of losing your configuration or identities (fms etc).

This image has Freenet auto-update disabled in favor of updating through Docker.

It is recommended that you continue to use it that way rather than turning on auto-update,
as there will be runtime changes that the Docker image will account for that may not be
handled otherwise.
