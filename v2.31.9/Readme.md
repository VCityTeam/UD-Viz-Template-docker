# Docker based demos of UD-Viz (version v2.31.9).

This directory holds the various docker based demos of [UD-Viz](https://github.com/VCityTeam/UD-Viz) version 2.31.9.
Each sub-directory holds a docker context to be used for building a docker image (container) of the demo considered demo and the instructions to run
the demo.

## Imuv

This demo illustrate an example of the UD-Viz game engine. 

Build the docker image with

```bash
docker build -t ud-viz:imuv Imuv
```

and run the container with

```bash
docker run [-d] -p 8080:80/tcp --rm -t ud-viz:imuv
```

and open a web browser on URL `http://localhost:8080/`

