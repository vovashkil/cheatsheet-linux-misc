# cheatsheet-linux-misc
### Change Default Network Name (ens33) to eth0 on Debian 10 / Debian 9

1. Change from
GRUB_CMDLINE_LINUX=""
to
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
in 
/etc/default/grub

2. grub-mkconfig -o /boot/grub/grub.cfg
3. change interfaces name to eth0, eth1, ...
4. reboot

### SSL certificate basic checks

#### SSL certificate end date
openssl x509 -enddate -noout -in server.crt
 
#### SSL certificate and prtivate key match
openssl x509 -noout -modulus -in server.crt | openssl md5

openssl rsa -noout -modulus -in server.key | openssl md5
