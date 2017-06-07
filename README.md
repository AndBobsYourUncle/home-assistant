# Complete Home Assistant in Docker
A way to get Home Assistant running in Docker on a Raspberry PI

## Installing
* Install Docker and Docker Compose on target machine
* Setup a NoIP account, and register any hostname you'd like.
* Set global environment variables for these things:
	* `HOME_ASSISTANT_HOST` (NoIP hostname)
	* `HOME_ASSISTANT_URL` (`https://HOME_ASSISTANT_HOST`)
	* `HOME_ASSISTANT_PASSWORD` (A very secure password)
	* `LETS_ENCRYPT_EMAIL` (Your email address to generate SSL)
	* `NOIP_USER` (Username for NoIP)
	* `NOIP_PASSWORD` (Password for NoIP)
* Run `docker-compose up -d`
* Set a Home Assistant password:
	* Run `sudo nano home-assistant/configuration.yaml`
	* Set the `api_password` to match the `HOME_ASSISTANT_PASSWORD` environment variable you chose.
* Run `docker-compose restart`

## Containers Used
This setup utilizes many docker images that I've forked and built so that it all runs on the Raspberry PI. Here's the info for each one, and their dependency chains. These will automatically get pulled during installation.

* `andbobsyouruncle/rpi-nginx-proxy`
	* Depends on `andbobsyouruncle/rpi-docker-nginx`
* `andbobsyouruncle/rpi-letsencrypt-nginx-proxy-companion`
* `andbobsyouruncle/rpi-home-assistant`
	* Depends on `andbobsyouruncle/rpi-python`
* `andbobsyouruncle/rpi-noip-docker`
* `andbobsyouruncle/rpi-homebridge-homeassistant`
