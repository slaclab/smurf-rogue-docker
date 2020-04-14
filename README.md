# Docker image with Rogue for the SMuRF project

## Description

This docker image, named **smurf-rogue** contains Rogue and additional tools used by the SMuRF project.

It is based on the **smurf-base** docker image and contains Rogue.

## Source code

Rogue source code is checked out for the SLAC's github repository https://github.com/slaclab/rogue.

## Building the image

When a tag is pushed to this github repository, a new Docker image is automatically built and push to its [Dockerhub repository](https://hub.docker.com/r/tidair/smurf-rogue) using travis.

The resulting docker image is tagged with the same git tag string (as returned by `git describe --tags --always`).

## Using the container

The image is intended mainly to be use as a base to build other docker images. In order to do so, start the new docker image Dockerfile with this line:

```
ROM tidair/smurf-rogue:<TAG>
```

A container however can be run as well from this image. For example, you can start the container in the foreground with this command

```
docker run -ti --rm --name smurf-rogue tidair/smurf-rogue:<TAG>
```

Where:
- **<TAG>**: is the tagged version of the container your want to run.

### Use the container to connect remote rogue GUIs:

The docker image includes an script called `connect_remote_gui.py` (available in the `PATH`) which can be used to connect a rogue GUI to a remote server.

This are the available options:
```
usage: connect_remote_gui.py [-h] [--host HOST_NAME] [--slot {2,3,4,5,6,7}]
                             [--atca-monitor] [--pcie] [--port PORT_NUMBER]

optional arguments:
  -h, --help            show this help message and exit
  --host HOST_NAME      Hostname where the server application is running.
                        Defaults to 'localhost'.
  --slot {2,3,4,5,6,7}  Connect a GUI to pysmurf server running on a AMC
                        carrier, located in this slot number. The port number
                        used is "9000+2*slot_number". Ignored if option "--
                        port" is used.
  --atca-monitor        Connect a GUI to an atca_monitor rogue server. The
                        port number used is 9100. Ignored if options "--slot"
                        or "--port" are used.
  --pcie                Connect a GUI to a PCIe card rogue server. The port
                        number used is 9102. Ignored if options "--slot", "--
                        port" or "--atca-monitor" are used.
  --port PORT_NUMBER    Connect to a rogue application running on a non-
                        predefined port number.
```

The server application must be already running before you try to connect a GUI to it. The remote server is identify by the hostname where it is running and the port number it is using. All
application types have predefined default port numbers:
- A `pysmurf server` application, running on an AMC carrier, uses by default port number `9000 + 2 * slot_number`,
- an atca-monitor application uses by default port number `9100`, and
- a pcie card application uses by default port number `9102`.

You can also use an specific port number if needed, using the `--port` option.

The hostname used by default is `localhost`, but you can specify a different one using the `--host` option.