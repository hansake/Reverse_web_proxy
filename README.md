# Reverse_web_proxy
This reverse web proxy is used as a gateway between Internet and web servers at home to monitor smart home functions.

This repository is now replaced by: [hansake/Reverse_web_proxy_LDAP: Reverse TLS web proxy with LDAP authentication](https://github.com/hansake/Reverse_web_proxy_LDAP)

## Design
The reverse proxy is implemented on a Raspberry Pi 3 B+ running Raspbian and the nginx web server.
Only TLS is supported from Internet and a username and password is mandatory to pass the gateway.

As implemented in this example the gateway is used to reach a heatpump monitor, a weather station with a weewx web interface,
an energy monitor and a Domoticz home control system.

To reach the different web servers at home, an index page with links was created.

Documentation for nginx reverse proxy: https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/

## Installation
Start with installing Raspian using NOOBS as described in: https://www.raspberrypi.org/downloads/noobs/

Then install nginx with the following steps:
* sudo apt-get update
* sudo apt-get install nginx-full

Test the installation by opening the IP address of Raspberry Pi in a web browser.

To create passwords and certificates openssl is used. This program is by default installed in Rasbian.

To create users and passwords it is handy to do this by using a script, see the file "addnginxuser".
The script was put the directory /usr/local/bin, copy the script to this directory and make it executable:
* sudo cp addnginxuser /usr/local/bin
* sudo chmod +x /usr/local/bin/addnginxuser

Then create a user and its password:
* sudo addnginxuser
* Enter new username: user name
* Enter new password: password
  
Create a self signed certificate
* sudo mkdir -p /etc/ssl/localcerts
* sudo openssl req -new -x509 -days 3650 -nodes -out /etc/ssl/localcerts/webgateway-cert.pem -keyout /etc/ssl/localcerts/webgateway-key.pem

The program will ask a number of questions, I put blanks in most places (by entering ".")
except for "Country Name" and "Common Name".

Set correct access rights of certificate files:
* sudo chmod 600 /etc/ssl/localcerts/*

The file "reverseproxy" is used to configure the web proxy.
Copy this file to the directory /etc/nginx/sites-available.
The configuration file will be enabled by making a soft link en the sites-enabled directory.

First remove the link to the default configuration:
* sudo rm /etc/nginx/sites-enabled/default

Then make a link to the reverse proxy configuration:
* sudo ln -s /etc/nginx/sites-available/reverseproxy /etc/nginx/sites-enabled/

Restart nginx to use the new configuration:
* sudo service nginx restart

Create an index file containing links to the web servers.
This file is named "index.html" and is copied to the directory /var/www/html.

Finally port forwarding must be configured on the Internet router at home.
The configuration is dependent on the type of router but port 443 shall be forwarded to
the IP address of the Raspberry Pi running the reverse proxy software.

Note that the router must be connected to Internet with a public IP address on the WAN side.
To reach the gateway from Internet, open https://your_public_IP in a web browser.
You have to convince the web browser that the self signed certificate is to be trusted.
This works a little bit different depending on the router you use.

## Future development

Some ideas in the wiki:
https://github.com/hansake/Reverse_web_proxy/wiki
