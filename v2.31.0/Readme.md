# Docker based demos of UD-Viz (version v2.31.0).
This directory holds the various docker based demos of 
[UD-Viz](https://github.com/VCityTeam/UD-Viz) version 2.31.0.
Each sub-directory holds a docker context to be used for building a docker 
image (container) of the demo considered demo and the instructions to run
demo.

### DemoFull
This demo illustrates all the 
[widgets](https://github.com/VCityTeam/UD-Viz/blob/master/Organisational_principles.md)
offered by the [UD-Viz library](https://github.com/VCityTeam/UD-Viz).

Build the docker image with
```
docker build -t ud-viz:demofull DemoFull
```

Retrieve your Fully Qualified Domain Name (FQDN). For example on OSX using
dhcp (and with a misconfigured/hardwired hostname) one can first use
`ifconfig  | grep -i inet` to retrieve the host IP number
and then `host <host_IP_number>` to retrieve the FQDN.

Run the container with
```
docker run -d -h <FQDN> -p 8080:80/tcp --rm -t ud-viz:demofull
```
and open a web browser on URL `http://localhost:8080/`

Notice that 
 * in the above `docker run` command the "-d" detached mode is not mandatory
 * the published port (the `8080` in the the above "-p" flag of the the 
   `docker run` command) can be changed but has to match with the port given
   (within the URL) when browsing
