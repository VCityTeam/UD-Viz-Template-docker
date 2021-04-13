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
ARG DOCUMENT_ROOT=/UD-Viz-demo/DemoFull

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

RUN git clone --branch v2.31.4 https://github.com/VCityTeam/UD-Viz-demo.git
WORKDIR $DOCUMENT_ROOT
RUN npm install
RUN npm run build 

######### Expose (through http) the 3DTiles tilesets 
# The tilesets are required by the demo. 
# References concerning the wget ad-hoc flags
# https://askubuntu.com/questions/719410/wget-web-crawler-retrieves-unwanted-index-html-index-files
RUN mkdir -p $DOCUMENT_ROOT/DataStore
WORKDIR $DOCUMENT_ROOT/DataStore
RUN apt-get install -y wget
RUN wget --recursive --no-parent --no-host-directories --cut-dirs=1 -R 'index.html?*' http://rict2.liris.cnrs.fr/DataStore/Lyon-2015-gltf-repaired/

######### Install apache2
# Although serving the site can be done by running `npm start` when on
# a desktop, it seems that using npm in a containerized context is not
# that simple, refer e.g. to https://github.com/iTowns/itowns/issues/1503
# The current workaround, that might be a little overkill, consists in
# using an independent httpd server to serve the JS/html code that was 
# build (refer above) with npm. 

# tzdata is an apache2 package dependency that requires some interactive
# configuration. The following is a non interactive workaround, refer to
# https://stackoverflow.com/questions/8671308/non-interactive-method-for-dpkg-reconfigure-tzdata/20693661
ARG DEBIAN_FRONTEND=noninteractive
ARG DEBCONF_NONINTERACTIVE_SEEN=true
RUN echo "tzdata tzdata/Areas select Europe" > preseed.txt
RUN echo "tzdata tzdata/Zones/Europe select Berlin" >> preseed.txt
RUN debconf-set-selections preseed.txt

RUN apt-get install -y apache2

######### Expose (through http) UD-Viz
# The site definition will expose the content of the above defined
# WebDemos directory
COPY dockerized_udv_demo.conf /etc/apache2/sites-available/

# Remove the default server (which is a symlink anyhow) to avoid collisions:
RUN rm -f /etc/apache2/sites-enabled/000-default.conf 

# Enable the virtual site
RUN a2ensite dockerized_udv_demo.conf

EXPOSE 80
# Launch httpd. Reference
# - https://stackoverflow.com/questions/44376852/how-to-start-apache2-automatically-in-a-ubuntu-docker-container
CMD [ "apache2ctl", "-D", "FOREGROUND" ]
