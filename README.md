# fifi
fifi cluster manual

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


