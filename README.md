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

To create passwords and certificates openssl is used. This program is by default installed in Rasbian.

To create users and passwords it is handy to do this by using a script, see the file "addnginxuser".
The script was created in the directory /usr/local/bin, copy the script to this directory and make it executable:
* sudo cp addnginxuser /usr/local/bin
* sudo chmod +x /usr/local/bin/addnginxuser

Then create a user and its password:
* sudo addnginxuser
* Enter new username: user name
* Enter new password: password
  
The file "reverseproxy" is used to configure the web proxy.
Copy this file to /etc/nginx/sites-available
The configuration file will be enabled by making a soft link en the sites-enabled directory.

First remove the link to the default configuration:
* sudo rm /etc/nginx/sites-enabled/default

Then make a link to the reverseproxy configuration:
* sudo ln -s /etc/nginx/sites-available/reverseproxy /etc/nginx/sites-enabled/

Last restart nginx to use the new configuration:
* sudo service nginx restart

