# Dockerfile for Tryton Server

This Dockerfile contains the steps required to build a working image of
Tryton server. The build is based on the official
[debian base image](https://registry.hub.docker.com/_/debian/) on 
[Dockerhub](https://docs.docker.com/docker-hub/repos/#repositories) and 
packages from [Debian Tryton Maintainers](http://tryton.alioth.debian.org/).

The image provided by this Dockerfile is a minimalistic Docker container
for Tryton server

* that works out of the box,
* follows [Best practices](https://docs.docker.com/articles/dockerfile_best-practices/) for Dockerfiles,
* and is meant to be extended to fit your personal needs. For further steps see below.

## Installation

If you don't have yet a running docker installation, install first `docker` with

    apt-get install docker.io

Add the user, that will use docker, to the docker group

    useradd myuser docker

Note: You may have to relogin before the group settings will take effect.


## Usage

Fetch the repository from docker

    docker pull mbsolutions/tryton-server

Note: To fetch and work with specific versions add the relative tag to the command like

		docker pull mbsolutions/tryton-server:3.4

Run a new container using the image

    docker run -d -p 8000:8000 mbsolutions/tryton-server

* The `-d` option indicates that the container should be run in daemon
  mode.
* The `-p` option and it's value `8000:8000` instructs docker to bind TCP port 8000
  of the container to port 8000 on the host. For more options on binding the interfaces
	of containers to the host machine see the
	[ports documentation](http://docs.docker.io/use/port_redirection/#port-redirection).


## Accessing the docker container

You can access the docker container and work from within it.

    docker exec -it mbsolutions/tryton-server /bin/bash

On execution of the command a new prompt within the container should be
available to you.

## Extending this image

This docker image is a minimal base on which you should extend and write
your own. The following example steps would be required to say
make your setup work with postgres and install the sale module.


    # Tryton Server with Sale module and Postgres
    #

    FROM mbsolutions/tryton-server:3.4
    MAINTAINER Mathias Behrle <mbehrle@m9s.biz>

		# Install additional distribution packages
		RUN apt-get update && apt-get install -y \
		tryton-modules-sale \
		&& rm -rf /var/lib/apt/lists/*
		
    # Get a [sample trytond.conf](https://alioth.debian.org/plugins/scmgit/cgi-bin/gitweb.cgi?p=tryton/tryton-server.git;a=blob;f=etc/trytond.conf;hb=refs/heads/debian-jessie-3.4),
		# copy it to the directory of your Dockerfile,
		# adjust the settings to your needs (e.g. connection parameters and credentials to your PostgreSQL server)
		# and copy it into the container with
    COPY trytond.conf /etc/tryton/trytond.conf

## Authors and Credits

This image was built at [MBSolutions](http://www.m9s.biz).

Parts of the setup were adopted from [tryton](https://github.com/openlabs/tryton) by [Openlabs](http://www.openlabs.co.in).

## Support

For any questions about this image and your docker setup contact our [support](mailto:info@m9s.biz).
