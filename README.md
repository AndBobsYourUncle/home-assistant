# Complete Home Assistant in Docker
A way to get Home Assistant running in Docker on a Raspberry PI. Everything is in Docker containers, so the Raspberry PI needs no installed dependencies besides Docker. 

This is a complete solution that will get you all set up and ready to install the Home Assistant iOS app, and start automating. You'll have a secure SSL connection to Home Assistant over the Internet, and a HomeKit bridge to allow any devices in Home Assistant to be controlled via Siri.

### Technology Stack

* Home Assistant
* Homebridge with Home Assistant plugin to make all devices in Home Assistant Siri-compatible
* Nginx, and a docker reverse proxy, to allow the Home Assistant to be reachable on a hostname
* LetsEncrypt for auto-generating SSL certificates for the docker container running Home Assistant
* NoIP docker container to automatically send your home's IP address to NoIP

## Installing
* Install Docker and Docker Compose on Raspberry PI:
	* Docker installation on PI: [RPI Docker Installation](http://blog.alexellis.io/getting-started-with-docker-on-raspberry-pi/)
	* Docker-Compose: [RPI Docker Compose Installation](https://www.berthon.eu/2017/getting-docker-compose-on-raspberry-pi-arm-the-easy-way/)
* Create the necessary Docker networks for the Docker Compose file:
	* `docker network create nginx`
	* `docker network create home-assistant`	
* Setup a NoIP account, and register any hostname you'd like.
* Set global environment variables for these things ([RPI Environment Variables](https://raspberrypi.stackexchange.com/questions/62548/setting-environment-variable-for-service):
	* `HOME_ASSISTANT_HOST` (NoIP hostname)
	* `HOME_ASSISTANT_URL` (`https://HOME_ASSISTANT_HOST`)
	* `HOME_ASSISTANT_PASSWORD` (A very secure password)
	* `LETS_ENCRYPT_EMAIL` (Your email address to generate SSL)
	* `NOIP_USER` (Username for NoIP)
	* `NOIP_PASSWORD` (Password for NoIP)
* Clone this repository and go into the cloned repository folder:
	* `git clone https://github.com/AndBobsYourUncle/home-assistant`
	* `cd home-assistant`
* Run `docker-compose up -d` (You will have to wait a long time for SSL certificates to be generated. You can check the status by running `docker logs lets-encrypt`)
* Set a Home Assistant password:
	* Run `sudo nano home-assistant/configuration.yaml`
	* Set the `api_password` to match the `HOME_ASSISTANT_PASSWORD` environment variable you chose.
* Run `docker-compose restart`
* Now you run `sudo nano home-assistant/configuration.yaml`, edit your Home Assistant configuration, and then run `docker-compose restart` anytime you need to have it add new devices/automations.

## Containers Used
This setup utilizes many docker images that I've forked and built so that it all runs on the Raspberry PI. Here's the info for each one, and their dependency chains. These will automatically get pulled during installation.

* `andbobsyouruncle/rpi-nginx-proxy`
	* Depends on `andbobsyouruncle/rpi-docker-nginx`
* `andbobsyouruncle/rpi-letsencrypt-nginx-proxy-companion`
* `andbobsyouruncle/rpi-home-assistant`
	* Depends on `andbobsyouruncle/rpi-python`
* `andbobsyouruncle/rpi-noip-docker`
* `andbobsyouruncle/rpi-homebridge-homeassistant`
