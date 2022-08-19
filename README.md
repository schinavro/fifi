# fifi
fifi cluster manual

### Multiple excutions

To do same execution throuout entire calculation nodes, do following.
```
~# for i in {1..40}; do ssh node$(printf "%02d" $i) echo < hostname; done
~# for i in {1..10}; do ssh c$(printf "%02d" $i) echo < hostname; done
```

## CPU 

### CPU INFO

`cat /proc/cpuinfo`

### CPU temperature check
To check the current cpu temperature (to check the the air cooling in the server room without going)

```
~# yum install lm_sensors
~# for i in {1..40}; do ssh node$(printf "%02d" $i) yum install -y lm_sensors
...
~# sensors

```

## Slurm

### Node drain / undrain
```
~# scontrol update partitions=G1 state=Drain reason=Maintenance
```

###
