# Reverse_web_proxy
This reverse web proxy is used as a gateway between Internet and web servers at home to monitor smart home functions.

## Design
The reverse proxy is implemented on a Raspberry Pi 3 B+ running Raspbian and the nginx web server.
Only TLS is supported from Internet and a username and password is mandatory to pass the gateway.

As implemented in this example the gateway is used to reach a heatpump monitor, a weather station with a weewx web interface,
an energy monitor and a Domoticz home control system.

To reach the different web servers at home, an index page with links was created.

## Installation
Start with installing Raspian using NOOBS as described in: https://www.raspberrypi.org/downloads/noobs/

Then install nginx with the following steps:
* sudo apt-get update
* sudo apt-get install nginx-full
Test the installation by opening the IP address of Raspberry Pi in a web browser.
