# EE-629-IoT

Pi-hole

https://pi-hole.net/ 

https://kylehelms.com/setting-up-pi-hole-on-an-old-desktop-with-ubuntu/ 

The site for Pi-hole, a useful software that monitors data traveling through a router and can block any requests.

Pi-hole allows us to control the network use of any application on any device on it. This means that we can block ads, data requests and
other unwanted transmissions. When paired with a VPN, you can control this for your network on the go as well.  Since less data is being
transmitted, it will improve performance on the network for all users. Pi-hole also provides a way to monitor the network and see how 
its statistics are holding up. 

1. To start we needed a supported operating system to run Pi-hole. Therefore, we installed Ubuntu on the laptop and allowed it to update
to the newest version. 

2. To make sure the laptop would not shut off if it was closed, I downloaded tweaks to change the setting. I used the command:
  
  sudo apt install gnome-tweak-tool

3. Next we installed Pi-hole by cloning their repository with the following command: 
  
  git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole

4.  Next we ran the setup from the folder "Pi-hole/automated install/" with the following command: 

  sudo bash basic-install.sh

5. This brings up the installation program. From here, I finished the set up and continued and with configuring what settings I wanted.

6. First, I continued through the normal logs to the upstream dns provider. The way the pi-hole works is any request to the router is of
an ip address that is on the list in pi-hole that includes blocked services, it will not execute the call. However, if it isn't in the 
list, the call will continue to the dns provider to find the destination. For my upstream dns server, I chose cloudfare since it seems
to be the fastest and it doesn't save information past 24 hours. 

7. Next we included all of the ad block lists that come with the program. These lists have the addresses of known IP addresses that
service ads and therefore their requests won't be completed. 

8. It then provides the current IP address and gateway for your system. It is important to set these as a static address so that the
router will always have it as the dns server. 

9. Then we install the web admin interface, the web server (lighttpd), and set it to log queries. This gives up complete control and
access to data from the router.  After this is completed, it finishes the installation and I reset the password for pi-hole. 

This is where problems began to occur which required a large amount of troubleshooting. There were several different problems that 
occurred which consist of:

- The original router that the home network was on was slow and would not update the DNS settings. The only solution to this problem was
to acquire a new router which we did. The router that was bought was the TP-Link AC1750 which was much easier to use to change its
settings.

- Once this router was set up, the original static IP address that was assigned to the computer was no longer viable as the default
gateway switched from 192.168.1.1 to 192.168.0.1. This meant the IP address would not be recognized by the router and therefore needed
to be reset. To do this, I reconnected the laptop to the router through ethernet and used the IP a command to find the new IP address
assigned to it. From there I used the earlier command to run the installation of Pi-hole and I kept the same settings except I
reconfigured the IP address. This solved the problem. 

- The new router has a feature called DNS rebind protection which meant that it wouldn't allow for the primary DNS server setting to be
changed to an IP address on a private network or this case the home network. The router did not have a setting to disable this so the
choice was made to set the Pi-hole as the DHCP server and set the primary DNS server there. Setting the Pi-hole as the DHCP server meant
that it would be assigning the IP addresses within the network to any devices that connected to the router instead of the router doing
so. The DHCP setting was disable on the router and enabled on Pi-hole which did not have DNS rebind protection.

- At this point, Pi-hole was up and running. It was blocking ads but some were getting through. This was because the IPv6 was not
configured on Pi-hole. This meant that it was not acting as the DNS server for IPv6 requests and they were getting through. To configure
IPv6, I looked up the current IPv6 address assigned to the laptop. From there I ran the command: 

  sudo nano etc/pihole/setupVars.conf
  
This opened the file setupVars.conf for editing which handled a majority of the configuration settings for the Pi-hole software. In this
file is a setting that should hold the IPv6 address that is assigned to the laptop but was actually empty and looked as such
IPV6_ADDRESS=2600:1700:. To fix this, The IPv6 address that was found earlier was added here and the file was saved. Then the Pi-hole
was restarted with the new address with the command pihole -r and it began to work correctly as shown here where it is now showing that
it is acting as the IPv6 DNS server. 
