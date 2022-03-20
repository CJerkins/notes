version: '3'
services:
  homeassistant:
    container_name: home-assistant
    image: homeassistant/home-assistant:stable
    ports: 
    	- 8123:8123
    volumes:
      - /PATH_TO_YOUR_CONFIG:/config
    environment:
      - TZ=America/New_York
    restart: always
    network_mode: host
	
	
version: '3'
services:
  homeassistant:
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - /opt/hass/config:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
	
	
