# Folder for Network Redundancy

# Reflection
The first thing I got set up was the networking. Working outward in I configured routing for xubuntu-wan, VyOS1, and VyOS2. Once I had the basic networking, including IPconfigs, basic NAT, and nameservers, I configured the virtual IPs to ensure simplicity and limit forgetfulness when transitioning. From there I set up the xubuntu-LAN box and routed it to the virtual IP. Luckily for me, it worked the first time. From there, I configured Web01 to exist on the LAN segment. Once I had a locally routable web page, remembering to open the local firewall, I set up NAT rules to forward port 80 to the WAN using the virtual IP. From there I tested the redundancy and capabilities of my network using ping, ssh, and a web browser. This worked. I installed google-authenticator on web01 for SSH and configured it in sshd_config. The biggest trial this project was not having touched a linux system in over a year. I found myself rusty and forgetting how to do basic things. Once I got into the swing of it, it was mostly smooth. I originally opted to put web01 in the OPT segment but this proved to be more trouble than it was worth at this current point so I moved it to LAN and proceeded. I will be troubleshooting the issues I had further but for now it works. Biggest lesson I learned this week is network management is not like riding a bicycle. You do forget things and need to brush up again and again. Command revisions make this harder but regular maintainence of knowledge would go a long way in this regard. 

## Web01 config
```bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --reload
```

## config1.txt
configurations for vyos1 after project 1

## config2.txt
configurations for vyos2 after project 1
