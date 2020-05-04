# EE-629-IoT

Pi-Hole

https://pi-hole.net/  

The site for Pi-hole, a useful software that monitors data traveling through a router and can block any requests.

Pi-hole allows us to control the network use of any app on any device on it. This means that we can block ads, data requests and other unwanted transmissions.
When paired with a vpn, you can control this for your network on the go as well.  Since less data is being transmitted, it will improve performance on the network for all users. 
Pi-hole also provides a way to monitor the network and see how its statistics are holding up. 

1. To start we needed a supported operating system to run Pi-hole. Therefore, we installed Ubuntu on the laptop and allowed it to update to the newest version. 

2. To make sure the laptop would not shut off if it was closed, I downloaded tweaks to change the setting. I used the command:
sudo apt install gnome-tweak-tool

3. Next we installed Pi-hole by cloning their repository with the following command: 
git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole

4.  Next we ran the setup from the folder "Pi-hole/automated install/" with the following command: 
sudo bash basic-install.sh

5. This brings up the installation program. From here, I finished the set up and continued and with configuring what settings I wanted. 

6. First, I continued through the normal logs to the upstream dns provider. The way the pi-hole works is any request to the router is of an ip address that is on the list in pi-hole that includes blocked services, it will not execute the call. 
However, if it isn't in the list, the call will continue to the dns provider to find the destination. For my upstream dns server, I chose cloudfare since it seems to be the fastest and it doesn't save information past 24 hours. 

7. Next we included all of the ad block lists that come with the program. These lists have the addresses of known ip addresses that service ads and therefore their requests won't be completed. 

8. It then provides the current ip address and gateway for your system. It is important to set these as a static address so that the router will always have it as the dns server. 

9. Then we install the web admin interface, the web server(lighttpd), and set it to log queries. This gives up complete control and access to data from the router.  After this is completed, it finishes the installation and I reset the password for pi-hole.
