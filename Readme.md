# UD-Viz-docker
This repo holds the docker context to be used for building docker images 
(containers) of the [UD-Viz](https://github.com/VCityTeam/UD-Viz) software.

Build the docker image
```
docker build -t ud-viz:2.25.0 v2.25.0
```

Retrieve your Fully Qualified Domain Name (FQDN). For example on OSX using
dhcp (and with a misconfigured/hardwired hostname) one can first use
`ifconfig  | grep -i inet` to retrieve the host IP number
and then `host <host_IP_number>` to retrieve the FQDN.

Run the container with
```
docker run -d -h <FQDN> -p 8080:80/tcp -t ud-viz:2.25.0 
```
and open a web browser on URL `http://localhost:8080/`

Notice that 
 * in the above `docker run` command the "-d" detached mode is not mandatory
 * the published port (the `8080` in the the above "-p" flag of the the 
   `docker run` command) can be changed but has to match with the port given
   (within the URL) when browsing