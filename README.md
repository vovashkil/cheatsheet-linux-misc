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
#### NB! Doing this (native vlan + tagged vlan in a bridge) on more than 1 node in a cluster will cause broadcast storm due to l2 loop
ip link add link bond0 name bond0.10 type vlan id 10

ip link set bond0.10 up

brctl addif vmbr0 bond0.10

ip addr add 10.10.10.10/24 dev vmbr0 label vmbr0:10

### DRBD Split Brain
#### Choose one node and run
drbdadm secondary all

drbdadm disconnect all

drbdadm -- --discard-my-data connect all

#### Make other node primary
drbdadm primary all

drbdadm disconnect all

drbdadm connect all

#### Check /proc/drbd again

### IPMI
ipmitool -I lanplus -H 10.10.10.10 -U ADMIN sol activate

### tcpdump
tcpdump -p -n -i external -s 1500 -l -x -X -c 1000000 -w /tmp/foo.dump

### zabbix server reset auth to default one if ldap is broken, default creds: Admin/zabbix
sudo -u zabbix psql

zabbix=>  update config set authentication_type=0 ;
 UPDATE 1

### Add Secondary IP Address To CentOS / RHEL Network Interface

/etc/sysconfig/network-scripts/ifcfg-eth0:1

DEVICE=eth0:1

Type=Ethernet

ONBOOT=yes

NM_CONTROLLED=no

BOOTPROTO=none

IPADDR=10.10.10.11

PREFIX=24
