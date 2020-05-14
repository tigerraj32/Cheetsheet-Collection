List of usefull command to monitor network bandwidth
[Ref Link](https://www.tecmint.com/linux-network-bandwidth-monitoring-tools/)

## List network interface

```bash
$ networksetup -listallhardwareports

Hardware Port: Wi-Fi
Device: en0
Ethernet Address: f8:ff:c2:03:3a:56

Hardware Port: Bluetooth PAN
Device: en6
Ethernet Address: f8:ff:c2:0d:34:ec
```

## Network Monitor

### slurm

**slurm** is a network load monitoring tool with color terminal graphs. It gives real-time traffic in terminal with three graph 
mode – combines RX and TX and two split views.

```bash 
$ brew install slurm
$ slurm -z -i en0         
```

### bmon

**bmon** (a.k.a. Bandwidth Monitor) is a network monitoring tool and is able to monitor multiple interface traffic. 
It gives data about packets, errors and whole lot of information needed for monitoring.

```bash
$ brew install bmon
$ bmon en0
```

