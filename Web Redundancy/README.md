# Folder for Web Redundancy

# Reflection
After brainstorming for half an hour I decided the best route for this would be to set up a VIP on the LAN for web devices. I did this using keepalived, setting the VIP to 10.0.5.200 and changing the NAT rules on the VYOS routers to that instead of web01. This just worked. The only issue I ran into was the keepalived config files. I had to delete everything but the VRRP block and change the interface. After I did this is was smooth sailing. 
## Web02 config
```bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --reload
```

I changed the default page to the hostname by editing /var/www/html/index.html and adding the hostname.

```bash
sudo yum install -y keepalived
sudo vi /etc/keepalived/keepalived.conf
```
I deleted everything except the following block. Edited it to these specifications.

```
vrrp_instance VI_1 {

        state MASTER
        interface ens192
        virtual_router_id 51
        priority 254
        advert_int 1
        authentication {
              auth_type PASS
              auth_pass 12345
        }
        virtual_ipaddress {
              10.0.5.200/24
        }
}
```

For Web02 I changed master to backup, everything else was the same.

## Vyos Changes
Set nat destination rule 100 translation address 10.0.5.200
