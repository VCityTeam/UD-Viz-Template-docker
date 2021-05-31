FROM ubuntu:focal

LABEL maintainer "VCityTeam"
LABEL source.repo "https://github.com/VCityTeam/UD-Viz-demo-docker"

RUN apt-get update

# The container will serve (through http) both:
#  1. the client code 
#  2. the data required by the demo
# Both (client and data) will be served by a single Appache2 http server
# configured to expose (refer to Apache2's DocumentRoot variable) the
# DOCUMENT_ROOT sub-directory.
ARG DOCUMENT_ROOT=/UD-Viz-demo/

######### Install the UD-Viz demo per se (client code) and build it
# Node version 14 is required. For its installation (on focal) refer to e.g.
#  https://computingforgeeks.com/install-node-js-14-on-ubuntu-debian-linux/
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
RUN node --version
RUN npm --version
RUN apt-get install -y git
RUN git --version

# The gm module invokes the convert binary through a system call.
# We must thus install imagemagick at the package level
# imagemagick package has dependency that requires some interactive
# configuration. The following is a non interactive workaround, refer to
# https://stackoverflow.com/questions/8671308/non-interactive-method-for-dpkg-reconfigure-tzdata/20693661
ARG DEBIAN_FRONTEND=noninteractive
ARG DEBCONF_NONINTERACTIVE_SEEN=true
RUN echo "tzdata tzdata/Areas select Europe" > preseed.txt
RUN echo "tzdata tzdata/Zones/Europe select Berlin" >> preseed.txt
RUN debconf-set-selections preseed.txt
RUN apt-get install -y imagemagick graphicsmagick

RUN git clone --branch v2.31.9 https://github.com/VCityTeam/UD-Viz-demo.git

######### Builds the client application
WORKDIR $DOCUMENT_ROOT/Imuv
RUN npm install
RUN npm run build-debug

######### Install the Imuv server 
# Note: the ImuvServer serves two things
#  1. the game server part (where avatars are, collissions...)
#  2. the http server for the client (ExpressJS based): previous build
#     in `Imuv` directory
WORKDIR $DOCUMENT_ROOT/ImuvServer
RUN npm install
RUN npm run build

EXPOSE 80
CMD [ "node", "./index.js", "../Imuv", "80" ]
