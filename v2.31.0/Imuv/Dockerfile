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
ARG DOCUMENT_ROOT=/UD-Viz-demo/Imuv

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

RUN git clone --branch v2.31.0 https://github.com/VCityTeam/UD-Viz-demo.git
WORKDIR $DOCUMENT_ROOT
RUN npm install
RUN npm run build-debug 

######### Install the Imuv server 
# Note: the ImuvServer serves two things
#  1. the game server part (where avatars are, collissions...)
#  2. the http server for the client (ExpressJS based)
WORKDIR $DOCUMENT_ROOT/../ImuvServer
RUN npm install
RUN npm run build

EXPOSE 8080
# Launch httpd. Reference
# - https://stackoverflow.com/questions/44376852/how-to-start-apache2-automatically-in-a-ubuntu-docker-container
# FIXME Add the port argument
CMD [ "node", "./index.js" ]
