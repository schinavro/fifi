# fifi
fifi cluster manual

### Password policy setting

```
[root@headnode~]# vim /etc/pam.d/system-auth
...
6  auth        required      /lib64/security/pam_tally2.so deny=5 unlock_time=10 no_magic_root
...
13 account     required      /lib64/security/pam_tally2.so no_magic_root reset
...
19 password    requisite     pam_pwquality.so try_first_pass local_users_only retry=8 authtok_type= minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1
...
[root@headnode~]# vim /etc/login_defs
#PASS_MAX_DAYS=90
#PASS_MIN_DAYS=1

```

### ADD SUDOERS

```
[root@headnode~]# visudo
root           ALL=(ALL)       ALL
<SUDOERS_ID1>  ALL=(ALL)       ALL
<SUDOERS_ID2>  ALL=(ALL)       ALL

[root@headnode~]# vim /etc/group

wheel:x:10:root, <SUDOERS_ID1>, <SUDOERS_ID2>

[root@headnode~]# vim /etc/pam.d/su
uncomment following lines
auth required pam_wheel.so use_uid

[root@headnode~]# chgrp wheel /bin/su
[root@headnode~]# chmod 4750 /bin/su
```

### PROXY SETTING FOR YUM
#### In Server Side
```
[root@headnode~]# yum install -y squid
[root@headnode~]# systemctl start, enable squid
[root@headnode~]# vim /etc/squid/squid.conf
acl localnet src 10.10.10.0/24          # nodes
acl localnet src 192.168.10.0/24        # nodes-nfs
acl SSL_ports port 443
acl Safe_ports port 80          # http
# acl Safe_ports port 21                # ftp
acl Safe_ports port 443         # https
...
http_access deny !Safe_ports
http_access allow localhost manager
http_access deny manager
...
http_access allow localnet
http_access allow localhost
...
http_access deny all

[root@headnode~]# systemctl restart squid
```
#### In Client Side
```
[root@node07~]# /etc/yum.conf
proxy=http://10.10.10.3128
```
### Re-mount node
```
[root@node13~]# umount /home, /appl, /data
[root@node13~]# mount -a
or
[root@node13~]# umount /home, /appl, /data
[root@node13~]# mount -t nfs headnode:/home /home
[root@node13~]# mount -t nfs headnode:/appl /appl
[root@node13~]# mount -t nfs headnode:/data /data
[root@node13~]# mount -t nfs cs01:/project /project

```

### Create account 

You need to 1. create the linux account 2. update NIS 3. update SLURM_DB 
```
[root@headnode~]# adduser <USER ID>
[root@headnode~]# passwd <USER ID>
...
...
[root@headnode~]# make -C /var/yp
[root@headnode~]# su <USER ID>
[USER_ID@headnode~]$ ssh-keygen -t rsa
...
[USER_ID@headnode~]$ cd ~/.ssh
[USER_ID@headnode~]$ cat id_rsa.put >> authorized_keys

```


### To reboot (2022/10/18)

Plug out the infiniband of `snoopy` headnode. Reboot the `node21~node40`. Replug the `snoopy` infinband cable.

### Multiple excutions

To do same execution throuout entire calculation nodes, do following.
```
[root@headnode~]# for i in {1..40}; do ssh node$(printf "%02d" $i) echo < hostname; done
[root@headnode~]# for i in {1..10}; do ssh c$(printf "%02d" $i) echo < hostname; done
```

## CPU 

### CPU INFO

`cat /proc/cpuinfo`

### CPU temperature check
To check the current cpu temperature (to check the the air cooling in the server room without going)

```
[root@headnode~]# yum install lm_sensors
[root@headnode~]# for i in {1..40}; do ssh node$(printf "%02d" $i) yum install -y lm_sensors
...
[root@headnode~]# sensors

```

## Slurm

### Node drain / undrain
```
[root@headnode~]# scontrol update partitions=G1 state=Drain reason=Maintenance
```

### Log
```
[root@headnode~]# cat /var/lib/slurm/slurmctld.log | grep error
...
[root@node01~]# cat /var/lib/slurm/slurmd.log | grep error
```

### UPDATE SLURM_DB

```
CHECK USER LIST
[root@headnode~]# sacctmgr list user -s 
ADD
[root@headnode~]# sacctmgr add user <USER_ID> account=phyiscslab
```

### modify the QOS
Example
```
[root@headnode~]# sacctmgr list qos 
[root@headnode~]# sacctmgr modify qos normal set maxtresperuser=cpu=120
```



