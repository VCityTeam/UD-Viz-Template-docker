# Docker based demos of UD-Viz (version v2.31.6).

This directory holds the docker based demos of [UD-Viz-Template](https://github.com/VCityTeam/UD-Viz).
Each sub-directory holds a docker context to be used for building a docker image (container) of the considered demo.

## DemoFull-Apache

This demo illustrates

- all the 
  [widgets](https://github.com/VCityTeam/UD-Viz/blob/master/Organisational_principles.md)
  offered by the [UD-Viz library](https://github.com/VCityTeam/UD-Viz),
- served to the client (i.e. to the web-browser) by an Apache2 httpd server.

Build the docker image with

```bash
docker build -t vcity/viz-template/demofull-apache DemoFull-Apache
```

Retrieve your Fully Qualified Domain Name (FQDN). For example on OSX using
dhcp (and with a misconfigured/hardwired hostname) one can first use
`ifconfig  | grep -i inet` to retrieve the host IP number
and then `host <host_IP_number>` to retrieve the FQDN.

Run the container with

```bash
docker run [-d] -h <FQDN> -p 8080:80/tcp --rm -t vcity/viz-template/demofull-apache
```

and open a web browser on URL `http://localhost:8080/`

Notes:

- in the above `docker run` command the optionnal `-d` argument requires the
  container to run in detached mode,
- the published port (the `8080` in the above `-p` flag argument of the the 
  `docker run` command) can be changed but has to match with the port given
  (within the URL) when browsing

## DemoFull-SimpleServer

This demo provides the same user experience as DemoFull but through the usage of
a different technical tool. Both DemoFull and SimpleServer demos illustrate all
all the
[widgets](https://github.com/VCityTeam/UD-Viz/blob/master/Organisational_principles.md)
offered by the [UD-Viz library](https://github.com/VCityTeam/UD-Viz).
But in order to so, DemoFull uses an Apache web server whereas SimpleServer
uses an [ExpressJS](https://en.wikipedia.org/wiki/Express.js) web-server.

Build the docker image with

```bash
docker build -t vcity/viz-template/demofull-simpleserver DemoFull-SimpleServer
```

and run the container with

```bash
docker run [-d] -p 8080:80/tcp --rm -t vcity/viz-template/demofull-simpleserver
```

and open a web browser on URL `http://localhost:8080/`
