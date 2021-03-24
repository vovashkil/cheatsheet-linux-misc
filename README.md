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

### Configure 802.1Q VLAN Tagging
ip link add link bond0 name bond0.10 type vlan id 10

ip link set bond0.10 up

brctl addif vmbr0 bond0.10

ip addr add 10.134.41.31/24 dev vmbr0 label vmbr0:10

### DRBD Split Brain
#### Choose one node and run
drbdadm secondary all

drbdadm disconnect all

drbdadm -- --discard-my-data connect all

#### Make other node primary
drbdadm primary all

drbdadm disconnect all

drbdadm connect all

##### Check /proc/drbd again

#### IPMI
ipmitool -I lanplus -H 10.10.10.10 -U ADMIN sol activate
