# Junos-ZTP

This repo provides instructions to do "Zero Touch Provisioning" for Junos Device on supported platform <br/>

Supported Junos platform in factory shipped config  have got enabled dhcp client on mgmt interface / revenu ports <br/>

In addition to dhcp clinet under the "chassis" stanza "auto-image-upgrade" is already enabled in factory shipped config <br/>

ZTP functionality can be enabled on supported on Junos platform by issue command "request system zeroize" <br/>

DHCP Server levearge on dhcp option 43 (subptions  0  for Junos image file name, 1 for  Junos config file) and 3 for for file transfer mode "i.e http/ ftp/ tftp" <br/>


DHCP option 150 provides IP address for  file server  <br/>

Junos device will download the image file and compare it with current Junos version and escape if Image file and running Junos version is same <br/>

Junos config file must be in curly bracket format and must have root-authenication command in it and new config will replace current config by using command load "override"  <br/>

For each device mgmt interface or chassis base mac+1 is required for ZTP  <br/>

Sample dhcpd.conf (/etc/dhcp/dhcpd.conf) can be used for quick setup <br/> 

Junos Config files and junos-image file should be placed in /var/www/html directory while using default config of httpd or apache2 web server config
