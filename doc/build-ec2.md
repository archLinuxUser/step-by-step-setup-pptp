# Instructions for building on an Amazon AWS EC2 server  
####    by Jeremy   http://archlinuxuser.info  
-------------------  
  
1. Start the Amazon EC2 server that was setup  
From the video [YouTube Video](https://www.youtube.com/watch?v=1hYPnUI9--c)  
Or you can use whatever Ubuntu-based Linux you may have (i.e. other VPS providers)  
 
2. Connect to the Amazon EC2 server  
ssh -i nameOfThePEMfile.pem ubuntu@ipaddress  (use putty on Windows)  

3. Install pptpd and git (git is already installed if using the previous EC2)
```
sudo apt-get update  
sudo apt-get install pptpd git
```

4. Make a working directory for git and clone the step-by-step-setup-pptp repo  
`cd ~ && mkdir git && cd ~/git`  
(working directory exists if using the previous EC2)  
`git clone https://github.com/archLinuxUser/step-by-step-setup-pptp.git`  

###Going to switch to root user for ease

5. Copy files from configFilesForPPTP/*  to  /etc/ppp/  (overwrite existing files)  
```
sudo su  
cd /etc/ppp  
cp -vr ~/git/step-by-step-setup-pptp/configFilesForPPTP/* ./  
```

6. Modify /etc/sysctl.conf to allow forwarding of IP packets
```
grep "net.ipv4.ip_forward" /etc/sysctl.conf  #show the line  
sed -i.original 's/^#net.ipv4.ip_forward/net.ipv4.ip_forward/' /etc/sysctl.conf  
#line above removes the # comment symbol  
grep "net.ipv4.ip_forward" /etc/sysctl.conf  #show the line again
sysctl -p  #make the change active and permanent
```  

7. Install iptables-persistent to save iptables changes
a. sudo apt-get install iptables-persistent

8. Test the PPTP VPN from your Android cell phone or Windws PC/laptop
```  
nano /etc/ppp/chap-secrets   #PLEASE change the username/password before using
wget -qO- http://whatismyip.org |grep -A1 "IP Address"   #IP address is on 2nd line
```  
screenshots of setting up [Windows](../screenshots/Windows/)
screenshots of setting up [Android Celfons/tablets](../screenshots/Android/)

9. Save a backup of the AWS EC2 instance  
a. Amazon AWS EC2 dashboard/instances  
b. Highlight/select the running instance, press on the button Actions, dropdown, create Image  
c. Name the image for use later
