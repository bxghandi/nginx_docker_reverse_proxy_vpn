# nginx_docker_reverse_proxy_vpn
Basic setup to run nginx reverse proxy for multiple docker containers routed through VPN docker container

## Supported Architectures

This setup uses qdm12's [private-internet-access-docker] (https://github.com/qdm12/private-internet-access-docker) image as the container providing the internet connection. All other images are from [linuxserver] (www.linuxserver.io). This will support all architectures supported by those images.

## Prerequisites 

A docker network for your vpn and connected containers. If you want to keep containers that do not route through the vpn separate from those that do, this method provides separation.

Run the following command:
'docker create network { name }'

## Links to Documentation

* [private-internet-access-docker] (https://github.com/qdm12/private-internet-access-docker) - VPN container
* [nginx] (https://hub.docker.com/r/linuxserver/nginx) - nginx
* [transmission] (https://hub.docker.com/r/linuxserver/transmission) - transmission

## Points of Interest

- This general framework will work with several setups. qdm12's docker container supports several VPN providers.
- This should work with services like [Radarr] (https://hub.docker.com/r/linuxserver/radarr) or [Sonarr] (https://hub.docker.com/r/linuxserver/sonarr) as long as the same general formatting found in the transmission docker-compose entry is used.
- This setup allows for webUI's for services to maintain functionality due to the nginx reverse proxy. For additional containers, just add on to nginx.conf in the the format of the transmission entry. 
- This functionally has a VPN kill-switch. If the VPN loses connectivity, your containers will also lose connection.
- Enter needed ports for your services container into the VPN docker-compose container, and not in the service container itself. 
- With one container acting as your router, you will not risk maxing out your device limit with your VPN.

## Setup

1. Create docker network for these containers
2. Configure docker-compose file in /pia folder and execute 'docker-compose up -d'.
3. Add the wanted services to the docker-compose file in the /reverse_proxy folder. Ensure each service has 'network_mode: "container:gluetun"'. If the service has a webUI, add the port to nginx.conf for the reverse proxy. With