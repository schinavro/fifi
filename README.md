# fifi
fifi cluster manual


### CPU temperature check

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
