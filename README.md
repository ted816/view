# Linux Notes

### DNS etc
/etc/hosts  
flush to resolve  
ip neigh flush all (flush DNS)  
ipp -s -s neigh flush all  
/etc/resolv.conf  
```
auto eth0
iface eth0 inet static
        address 192.168.1.100
        netmask 255.255.255.0
        gateway 192.168.1.1
        dns-nameservers 8.8.8.8 8.8.4.4
```
systemctl restart NetworkManager (external app)


## VPN Tools
tailscale, wrieguard  

## Remote GUI Tools
VNC, XRDP  

## Management and IAC tools
Terraform, Ansible, Chef, Puppet, AWS Cloud formation  

## Logging Tools
EFK - elastic, Fluent, Kibana  
PLG - Promtail, Loki, Grafana  
FIG - Fluent, InfluxDB, Grafana  

log aggregation - loki  
monitoring - Prometheus, grafana  
independent - rsyslog  

## Mail Tools
Mailx, postfix  

## Commands
### Common
|Code|Use|
|:---|:---|
|ls -ltur | long, time, last used, reverse |
|ed | edit file name|
|pr | print in pages|
|tail -n5| print last 5 line|
|cmp file1 file2| print location of first diff|
|diff file1 file2| print all difference|
|ls > file.txt | use >> for append |
|file file_name| show file format|
|state file_name| show detail access and others |
|ps and top | show process running |    
|nmcli | better ip a, or use nmtui change hostname (gui)|
|hostnamectl | show host information |
|wg genkey l tee privatekey l wg pubkey > publickey ||
|sysctl net.ipv4.ip_forward=1 | /proc/sys/net/ipv4/ip_forward|


### uncommon
|Code|Use|
|:---|:---|
|last, lastlog, who, w | Login information | 
|stat -c "%a %n %s %F"| custom infomation|
|stat -L file | stat -f or stat -L too|
|chown | change owner/group|
|chattr | immutable flag |
|getfacl | see permission |
|chmod +t / or chmod 1777 | sticky bit, only owner can delete/rename|
|ln | hard link and soft link a file |
|find, grep, awk, sed | google for more parameters |
|tar | zip compress |
|dnf history | show history of dnf | 
|rpm -qa | show all packages | 
|fdisk /dev/sda| parition disk (fdisk -l /dev/)|
|cat /proc/partitions| show partition disk|
|du -sh /directory| show folder size|
|lspci, lsblk, lscpu, lsusb | ls others |
|recnice | change process priority |



### directory
|Code|Use|
|:---|:---|
|/etc| configuration files|
|/var/log| system and service log file|
|/usr | multi user utilites and application|
|/var | variable file|
|/proc| kernel and process (cat here for others, cpu, version, etc)|
|/svr | services |
|ls -l /usr/bin l grep programme_name | check programe name|
|/var/log/secure | what root access is doing |
|/etc/logroate.conf | how long log is cleared |


# Network
## Security Network Connection Related CLI
iptables - packet filtering (older firewall)  
nftables(newer firewall)  
firewalld (RHEL firewall)  
tcpdump and wireshark (splunk, ELK, Velociraptor)  
mxtoolbox.com - DNS lookup website  
bgp toolkit (hurricane electruc) - IP website and toolkit  
looking glass for traceroute from SG bridging point  

|Ping||
|:---|:---|
|ping -c 3 -n ip | remove hostname only ip|
|ping -i 0.5 -s 1024 ip | interval and size |
|ping 0 | loopback|
|sudo ping -f ip | flood ping |


|Connection and IP address| |
|:---|:---|
|nmcli | show ip and info|
|nmcli device show | show ip and info |
|nmcli device show enp0s | show ip and info |
|nmcli -p device | show info |
|nmcli general status | show connectivity |
|ifconfig | good associate |
|ip route show | can associate|
|ip a | associate |
|nmtui | edit interface |

|Hostname||
|:---|:---|
|hostnamectl | detailed information |
|hostname | hostname|
|cat /etc/hostname | hostname|

|NAT||
|:---|:---|
|arp| system arp cache (can manual add) |
|ip n | system arp cache |
|cat /proc/net/arp| show arp|
|namp -sn 192.168.31.0/24 | scan device on router subnet + nmcli |
|sudo ip n flush all | flush nat (can manual add) n or neigh |

|Port and Discovery||
|:---|:---|
|nmap -p 22 subnet | scan subnet for port 22|
|nmap host | scan opened port on host |
|nmap -sn subnet | host discovery with ping |
|nmap -O host | operating system |
|nmap -sL subnet | no send packet discover|

|netstat open tcp connections||
|:---|:---|
|ss -tnulpa | tcp numeric udp listening process all|
|ss -s | summary | 
|netstat -tnpe | tcp numeric(https-443) program extra_info|
|netstat -u -a | udp or no -u to show all or -a(all)|
|netstat -i -r | interface and route |

|route finding||
|:---|:---|
|traceroute | trace route|
|mtr | ping type, good for multiple layer |
|tracepath | mine failed on google, test MTU, UDP port, but nice format|
|mtr -r -c 5 host > file.txt | report mode, have csv n json flag |

|DNS||
|:---|:---|
|cat /etc/resolv.conf | DNS config |
|nmcli  | show DNS config |
|host| look for server DNS |
|dig | look for server DNS |
|nslookup | look for server DNS |

|Routing for VPN||
|:---|:---|
|route||
|netstat||

|tcpdump||
|:---|:---|
|tcpdump -D | show device|
|tcp -i ens0p -tnv -rw file | interface, time, numeric, verbose, read/write|
|and ! host 192.0.0.0 | keep and |

Process

|System information||
|:---|:---|
|dmidecode l grep -A3 '^System Information'| specific |
|lsof | list of file open, much config |


## setting interface or iptable
/etc/network/interfaces  
static static ip  
> allow-hotplug ens18  
iface ens18 int static  
address 192.168.31.xxx  
gateway 192.168.31.1  

bridge setting  
>allow-hotplug ens19  
iface ens19 inet dhcp  

default low  
> auto lo  
iface lo inet loopback  

static bridge  
> auto vmbr0  
iface vmbr0 inet static  
address 192.168.31.100/24  
gateway 192.168.31.1  
bridge-ports eno1  
bridge off  
bridge-fd 0  
netmask 255.255.255.0 (optional)  

refreshing / update  
ifdown ens18  
ifup ens18  
systemctl restart networking  
note  
auto ens18 (start on boot)  
hotplug ens18 (start on plugged)  
MAC address vs (ip a), the mac address should tally  
-Multiple ip in one interface example (not tested)  
auto ens99  
allow-hotplug ens99  
iface ens00 inet static  
address 123/24  
gateway 123  
iface ens99 inet static  
address 124/24  
iface ens99 inet static  
address 124/24  

## Wireguard

|Configuration||
|:---|:---|
|wg keygen > private | wg keygen > private|
||cat private|
||wg pubkey < private |
|ip link add wg0 type wireguard| ip link add wg0 type wireguard|
|ip addr add 10.0.0.1/24 dev wg0| ip addr add 10.0.0.2/24 dev wg0|
|wg set wg0 private-key ./private| wg set wg0 private-key ./private|
|ip link set wg0 up|ip link set wg0 up|
|wg set wg0 listen-port 51820||
|ip 192.168.1.1 wg0 10.0.0.1 | ip 192.168.1.2 wg0 10.0.0.2|
|wg set wg0 peer his_pub_key allowed-ips 10.0.0.2/32 endpoint 192.168.1.2:51820| wg set wg0 peer his_pub_key allowed-ips 10.0.0.1/32 endpoint 192.168.1.1:51820|
|ping 10.0.0.2||

### Sample
```
VYOS Setting
    wireguard wg03 {
        address "10.1.0.33/30"
        peer toh02 {
            allowed-ips "0.0.0.0/0"
            public-key "+====="
        }
        port "51850"
        private-key "c="
    }
	wireguard_in
       rule 30 {
                action "accept"
                description "phone"
                destination {
                    port "51850"
                }
                protocol "udp"
            }

WG0.config
[Interface]
Address = 10.1.0.2/30
ListenPort = 51850
PrivateKey = 6KdEKgcExxNtWxGQwLDX61fPIH9mKGVCCnDkrVMUqG0=
DNS = 192.168.16.35

[Peer]
PublicKey = EQY9BtHO6fRvRTZ21/3ig3i9JbcgpEc2IjkkNDvXI0Q=
AllowedIPs = 10.1.0.0/30, 192.168.16.32/27
Endpoint = 192.168.31.101:51820
PersistentKeepalive = 15 # Optional: keep connection alive
```

## DNS

|Record Name|Description|Use|
|:---|:---|:---|
|A| Address, show IP of a domain or host|IP address lookup using domain name|
|AAAA|Same as A, but IPv6| Same as A|
|CNAME|points to domain name(alias) to another domain|ng.example.com points to example.com points to the actual IP address using A record|
|NS|Name server specify authoritative DNS server for a domain|Where web can find the ip for a domain name|
|MX|Mail exchange, where emails for a domain should be routed to, multiple MX record for domain is normal, eg backup|Name @ Type MX Priority 10 or 20 RDATA mx.example.com|

|Record Name|Use|
|:---|:---|
|SOA|start of authority|
|TXT|text|
|PTR| pointer, domain name for reverse lookup|
|SRV|store Ip and port for specific service|
|CERT|stores pub key and certificate|
|DCHID|DNS record store info related to DHCP|
|DNAME|delegation name, similar to CNAME, but it points all subdoamins for the alias to canonical domain name, while CNAME is point 1 sub domain to alias(shouldbe)|

### bind software
```
/etc/bind/named.conf.options
options {

        auth-nxdomain yes;
        directory "/var/cache/bind";
        //directory "var";

        allow-query {
            192.168.37.0/24;
            10.1.0.0/30;
        };

        forwarders {
            192.168.37.1;
        };
        listen-on port 53 {
            192.168.37.13; 127.0.0.1; ::1;
        };

};

zone "localhost" {
        type master;
        file "localhost.zone";
};

zone "0.0.127.in-addr.arpa" {
        type master;
        file "0.0.127.zone";
};

zone "fst.com" {
        type master;
        file "fst.com.zone";
};

logging {

};
```
```
/var/cache/bind/fst.com.zone
$TTL 3D

$ORIGIN fst.com.

@       1D      IN     SOA     @       root (
                       2013050101      ; serial
                       8H              ; refresh
                       2H              ; retry
                       4W              ; expiry
                       1D              ; minimum
                       )

@       IN      NS      @
        IN      A       127.0.0.1
bind    IN      A       192.168.37.13
ice     IN      A       192.168.37.1
web1    IN      A       192.168.37.19
web2    IN      A       192.168.37.20
```
```
root@bind:/var/cache/bind# cat 0.0.127.zone 
$TTL 3D

@       IN      SOA     localhost. root.localhost. (
                        2013050101      ; Serial
                        8H              ; Refresh
                        2H              ; Retry
                        4W              ; Expire
                        1D              ; Minimum TTL
                        )

       IN      NS      localhost.

1      IN      PTR     localhost.
root@bind:/var/cache/bind# cat localhost.zone 
$TTL 3D

$ORIGIN localhost.

@       1D      IN     SOA     @       root (
                       2013050101      ; serial
                       8H              ; refresh
                       2H              ; retry
                       4W              ; expiry
                       1D              ; minimum
                       )

@       IN      NS      @
        IN      A       127.0.0.1
bind    IN      A       192.168.37.13
```
```
root@bind:/var/cache/bind# cat managed-keys.bind
$TTL 3D ; 0 seconds
.                       IN SOA  . . (
                                10         ; serial
                                8H          ; refresh (0 seconds)
                                2H          ; retry (0 seconds)
                                4W          ; expire (0 seconds)
                                1D          ; minimum (0 seconds)
                                )
                        KEYDATA 20250930080858 20250929042250 19700101000000 257 3 8 (
```
```
#split DNS in vyos
    dns {
        forwarding {
            allow-from "192.168.37.0/24"
            cache-size "0"
            domain fst.com {
                name-server 192.168.37.13 {
                }
            }
            listen-address "192.168.37.1"
        }
    }
```


# Server Automation 

## Ansible
/var/lib/vz/template/iso  
Permission: API, Token\ ID is description  
apt-get install ansible  
apt-get install python3-pip (dont need)  
pip install proxmoxer (dont need)  
apt-get install python3-proxmoxer(didnt install 2nd time)  
apt-get install python3-pip and python3-proxmoxer (updated)  

use proxmox_kvm instead of community.proxmox.proxmox (updated)\ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible_key  
ssh-copy-id -i ~/.ssh/ansible_key.pub user@remote_host_ip  
```ini
hosts.ini
[myhosts]
192.168.31.10 ansible_python_interpreter=/urs/bin/python3 ansible_user=root ansible_ssh_private_key_file=~/.ssh/ansible_key
```

ansible_key  
  
ansible myhosts -m ping -i inventory.ini  
-vvvv (for debugging)  

```yml
test.yml
---
- name: Deploy Proxmox VM from Template
  hosts: localhost # Run from the Ansible control node
  connection: local
  gather_facts: false

  vars:
    proxmox_host:  # Replace with your Proxmox host IP or host>
    proxmox_api_user: root@pam # Replace with your Proxmox API user
    proxmox_api_token_id: ansible-token # Replace with your Proxmox API tok>
    proxmox_api_token_secret: fd7d6eee- # Secret Code
    template_vmid: 9000 # Replace with the VMID of your template
    new_vm_name: my-new-vm
    new_vm_id: 400 # Desired VMID for the new VM
    target_node: pve-node1 # Proxmox node to deploy the VM on
    storage_pool: local-lvm # Storage pool for the new VM's disk

  tasks:
    - name: Start the new VM
      proxmox_kvm:
        api_host: "{{ proxmox_host }}"
        api_user: "{{ proxmox_api_user }}"
        api_token_id: "{{ proxmox_api_token_id }}"
        api_token_secret: "{{ proxmox_api_token_secret }}"
        vmid: "{{ new_vm_id }}"
        node: br2no0
        name: tohs
        state: started
```





# Hypervisor
Have 2 HDD, one for OS, the other data all in HDD  
24k annual vcenter  

## Proxmox
/var/lib/vz/template/iso (iso directory)  
apt-get install --reinstall apt (repo error)  
apt-get install open-vm-tools (show ip)  
apt-get install net-tools (arp etc)  
qm list  
Remove enterprise linux  
etc/apt/  
deb http://download.proxmox.com/debian/pve <YOUR_DEBIAN_VERSION> pve-no-subscription  
qm list  
qm start 200  

```
CGNAT setting
auto lo
iface lo inet loopback

iface enp1s0 inet manual

iface wlp0s20f3 inet manual

auto dummy0
iface dummy0 inet auto
        pre-up ip link add dummy0 type dummy

auto vmbr0
iface vmbr0 inet static
        address 192.168.37.10
        gateway 192.168.37.1
        bridge-ports dummy0
        bridge-stp off
        bridge-fd 0

auto vmbr1
iface vmbr1 inet dhcp
        bridge-ports enp1s0
        bridge-stp off
        bridge-fd 0

source /etc/network/interfaces.d/*
```


## xcp-ng
using local iso  
xe sr-create name-label="LocalISO" type=iso device-config:location=/var/opt/xen/ISO_Store device-config:legacy_mode=true content-type=iso  
xe sr-list  
https://github.com/Jarli01/xenorchestra_installer  

## Others
Harvester -free but 32GB Ram  
Hyper-V - seems type 2  
AHV/Nutanix - need corporate email  
Ovirt - memory and update issue  
RHV - EOL soon  
Proxmox - Fragile but easy and fast  
OpenNebula - Monitoring, not hypervisor  
OLVL - avoid ORACLE  
vmware/Esxi/HyperV - $80-$180 per core for entry  
xcp-ng - Xen  
XenOrchestra - pay to use  
QEMU/KVM - baremetal no gui  


# Linux Uncommon

## RAM
RAM - physical memory used by active program to access data  
swap - use portion of your hdd to acts as virtual memory  

# Module
module is a loadable component that extens the functionality of the linux kernel without rebuild  
lsmod, lmod, modinfo  

# Daemons and service
Daemons - non interactive programs with no direct communication with users  
Service - interactive programs that communicate with users  
.target - core components of systemd, managing system startup process  

# Crontab
Execute crontab at a specific schedule  


# Shell
linux command
|Code|Use|
|:---|:---|
|date +%V | get date, ISO week |
|w | get user online |
|uname --help | some system|
|uptime| uptime |
|printf "hello\n" | print|
|echo "hello" | print |
|which -a script_name|check bash script|
|whereis script_name||
|./script name|run script or debug|
|rbash name||
|sh name||
|bash -x name|debug breakdown|
|source name| execute on current shell, variable will remain|
|set -x|check other set for debug|
|set +x|write in script for debug|
|echo '$USER'| out $USER|
|echo "$USER" | out actual_user|
|echo `date` | execute date|
|echo $(date) | execute date|
|echo ${variable:=123}| echo variable 123, out 123|
|echo $[1+2+3]| output 6|



variable
|Code|Use|
|:---|:---|
|$USER| user|
|$HOME| /home/user/ directory, different from user and root|
|$PWD||
|note| check "export -p" "printenv"|
|cat < filename | input file to use cat, display filename |

storage
|Code|Use|
|:---|:---|
|export MY_VARIABLE="Hello world"| export variable|
|my_function(){echo "hello world"} export -f my_function | export function|
|export -p| show variable|
|export -n MY_VARIABLE| remove funiction|
|note| any child can call function|

grep
|Code|Use|
|:---|:---|
|grep ^root file_name | line starting with root|
|grep  [yf] file_name | line with y or f|
|grep '\<c...h\>' file_name | start with c xxx end h|
|grep '\<c.*..*h\>' file_name | start with c end h|

sed
|Code|Use|
|:---|:---|
|sed '/error/p' file_name|print file plus print line with error, line with error printed twice|
|sed '/error/d' file_name|print line without error word, aka delete|
|sed -n '/error/p' file_name|print line with only error|
|sed -n '/^this/p' filename||
|sed -n '/^this.error/p' filename||
|sed -n '/^this.error$/p' filename||
|sed 's/error/pokemon/' filename|replace error with pokemon|
|sed 's/error/pokemon/g' filename| replace all error, previous is till the first error|
|sed 's/^/> /' filename| start of all line add >|
|sed 's/$/ OMG/' filename| end of all line add OMG|
|sed -e 's/x/y/g' -e 's/w/z/g' filename| use 2 replacement|

awk
|Code|Use|
|:---|:---|
|ls -l l awk '{print $1 $2}' | print column 1 and 2|
|sort, head | go c man|

Init, the initial process, reads its configuration files and decides which services to start or stop in each run level  

alias 123456='echo "helloworld"'  
123456  
helloworld  
unalias 123456  


```
#! /bin/bash
if [ "$USER" = "root" ];
then
        echo "root user"
else
        echo "$USER"
fi
a=1
b="hello world"
echo "no space for declare variable $a $b"

program > /dev/null 2>&1 
# 0 - stdin
# 1 - stdout
# 2 - stderr
```
```
#! /bin/bash

num=5
if [ $# -lt 3 ] # lt is less than, use $1, $# or num
then
#ge, greater equal
echo "this less than 3"
else
echo "this is more than 3"
fi

if [$# -lt 1 ]
then
echo "error"
exit 1
fi
yum install $1 << confirm
y
CONFIRM
```
```
ls *.xml
ls *.xml > list
for i in 'cat list'; do cp "$i" "$i".bak; done
ls *.xml*
```

<!-- ####################################################################
#########################################################################
#########################################################################
#########################################################################
#########################################################################
############ THINGS UNLIKELY TO USE AGAIN ###############################
#########################################################################
#########################################################################
#########################################################################
#########################################################################
######################################################################-->

# Others

## suricata
Specific command to get object.  
suricata-update  
systemctl restart suricata  
lspci -D | grep 'Network\|Ethernet'  
cat eve.json | jq -c 'select(.event_type=="flow")|[.src_ip,.dest_ip]'  
cat eve.json | jq -c '[.src_ip,.dest_ip,.proto,.dest_port]'  
cat eve.json | jq .  
zcat eve.json.1.gz | jq.  

### WAN 35
cat eve.json | jq -c 'select((.proto!="IPv6-ICMP") and (.dest_port!=5353)) |[.src_ip,.dest_ip,.proto,.dest_port]'  
cat eve.json | jq -c 'select((.proto!="IPv6-ICMP") and (.dest_port!=5353) and (.dest_port!=5355) and (.dest_port!=123)) |[.src_ip,.dest_ip,.proto,.dest_port,.timestamp]'  
cat eve.json | jq -c 'select((.proto!="IPv6-ICMP") and (.dest_port!=5353) and (.dest_port!=5355) and (.dest_port!=123) and (.dest_port!=1900) and (.dest_port!=67) and (.dest_port!=68)) |[.src_ip,.dest_ip,.proto,.dest_port,.timestamp]'  
cat eve.json | jq -c 'select((.proto!="IPv6-ICMP") and (.dest_port!=5353) and (.dest_port!=5355) and (.dest_port!=123) and (.dest_port!=1900) and (.dest_port!=67) and (.dest_port!=68) and (.dest_ip!="255.255.255.255") and (.dest_ip!="192.168.31.255") and (.dest_ip!="239.255.255.250")) |[.src_ip,.dest_ip,.proto,.dest_port,.timestamp]'  

### LAN 37
cat eve.json | jq -c 'select((.proto!="IPv6-ICMP") and (.dest_port!=5353)) |[.src_ip,.dest_ip,.proto,.dest_port,.timestamp]'  
<!---show stats (null) and SSH-->
34.107.243.93 - Google LLC  


## harddisk
apt install ntfs-3g  
sudo apt install nfs-common  
sudo apt install cifs-utils (no use)  
sudo ntfsfix -d /dev/sdb1  

## Psono Docker
```
sudo ipa-getcert request \
  -f /etc/pki/tls/certs/psono.crt \
  -k /etc/pki/tls/private/psono.key \
  -D psono.example.com \
  -K HTTP/psono.example.com
```
ss -tuln | grep 5432  
docker network create my-network  
docker run --network my-network  
ipa-getcert list  
ipa-getcert stop-tracking -i 20260705122004  
nginx -t  
docker ps  
docker restart <ID>  
start nginx  
sudo setsebool -P httpd_can_network_connect 1  
sudo systemctl reload nginx  
docker exec -it postgres psql -U postgres <into db>  
sudo find / -name "postgresql.conf" 2>/dev/null  


## Freeipa
ipa-server-install --allow-zone-overlap  
ipa dnsconfig-mod --forwarder=192.168.16.0 <firewall>  
ipa dnsconfig-show  
/etc/named/ipa-ext.conf  
acl trusted, 127.0.0.1, 192.168.16.0/32;  
firewall-cmd --list-services  
firewall-cmd --permanent --add-service=dns  
firewall-cmd --reload  
sss_cache -E  
systemctl restart sssd  

ipa-client-install --domain=xx.com --mkhomedir  
hostnamectl set-hostname user.xx.com  
dnf install freeipa-client  
