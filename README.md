# Junos-ZTP

This repo provides instructions to do "Zero Touch Provisioning" for Junos Devices on supported platform <br/>

Supported Junos platform in factory shipped config  have got enabled dhcp client on mgmt interface / revenu ports <br/>

In addition to dhcp client config ; under the "chassis" stanza "auto-image-upgrade" is already enabled in factory shipped config <br/>

ZTP functionality can be enabled on supported  Junos platform by issuing the command "request system zeroize" <br/>

DHCP Server levearge on dhcp option 43 (subptions  0  for Junos image file name, 1 for  Junos config file) and 3 for for file transfer mode "i.e http/ ftp/ tftp" <br/>


DHCP option 150 provides IP address for  file server  <br/>

Junos device will download the image file and compare it with current Junos version and escape if Image file and running Junos version are same <br/>

Junos config file must be in curly bracket format and must have root-authenication command in it <br/>

New config will replace current config by using command "load override"  <br/>

For each device;  mgmt interface or chassis base mac+1 is required for ZTP  <br/>

# Execution

yum install httpd dhcp -y <br/>

Edit the /etc/dhcp/dhcpd.conf file (use sample file in this repo but please do necessary changes as per your enviornment <br/>

Junos Config files and junos-image file should be placed in /var/www/html directory while using default config of httpd or apache2 web server config <br/>

Make sure SELinux type context is applied on files placed inside /var/www/html directory <br/>

ls -Z /var/www/html  <br/>

-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 dcg1.conf <br/>
-rw-r--r--. root root unconfined_u:object_r:admin_home_t:s0 dcg2.conf #incorrect SELinux type context and this file can not be accessed by httpd <br/>
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 leaf1.conf <br/> 
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 leaf2.conf <br/> 
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 spine1.conf <br/> 
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 spine2.conf <br/> 

Enable dhcpd and httpd daemons:- <br/>
  * systemctl enable dhcpd --now 
  * systemctl enable httpd --now 

Verify the status of dhcpd and httpd daemons by using sysemctl status command <br/> 

If Junos devices has required config in place then ZTP process should have start now <br/>

Relevant Logs files inside Junos devices are:- <br/>
  * /var/log/image_load_log <br/>
  * /var/log/op-scripit.log 
