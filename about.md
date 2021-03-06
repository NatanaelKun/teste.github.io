---
layout: page
title: Manual Installation
permalink: /Documentation/
---

## Hardware Requirements:
    - An architecture which supports Intel Cache Monitoring Technology (CMT)

##### For more information about CMT click [here](https://software.intel.com/en-us/blogs/2014/12/11/intel-s-cache-monitoring-technology-software-visible-interfaces)

## Software Requirements:
    - Ubuntu Server 16.04.2
    - linux kernel 4.4.0-75
    - dbgsym
    - make
    - g++
    - python
    - systemtap - 3.1
    

##### Download Ubuntu Server 16.04.2: [[download]](http://old-releases.ubuntu.com/releases/16.04.2/ubuntu-16.04.2-server-amd64.iso)

##### Installing Linux Headers: 

```shell
apt-get install linux-image-4.4.0-75-generic
apt-get install fdutils linux-source-4.4.0 linux-headers-4.4.0-75-generic
```

##### Installing dgbsym:

```shell
echo "deb http://ddebs.ubuntu.com $(lsb_release -cs) main restricted universe multiverse
deb http://ddebs.ubuntu.com $(lsb_release -cs)-updates main restricted universe multiverse
deb http://ddebs.ubuntu.com $(lsb_release -cs)-proposed main restricted universe multiverse" | \
sudo tee -a /etc/apt/sources.list.d/ddebs.list

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 428D7C01 C8CAB6595FDFF622

sudo apt-get update

sudo apt-get install linux-image-4.4.0-75-generic-dbgsym

```
##### Installing make, g++ and python:
```shell
sudo apt-get install make g++ python
```

##### Installing systemtap:
Download systemtap 3.1 [[download file]](https://sourceware.org/systemtap/ftp/releases/systemtap-3.1.tar.gz)

```shell
wget https://sourceware.org/systemtap/ftp/releases/systemtap-3.1.tar.gz
tar -xvzf systemtap-3.1.tar.gz

cd systemtap-3.1
sudo apt-get install gettext
apt-get install libdw-dev

./configure
sudo su
make
make install

rm -rf /root/.systemstap/*.*
reboot
```

#### Installation:

In order to test the whole installation package:

```shell
 stap  -e  ’probe  begin   printf(”Hello,  World!”)’; 
```

#### Downloading IntP:
```shell
function gdrive_download () {
 CONFIRM=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate "https://docs.google.com/uc?export=download&id=$1" -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')
 wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$CONFIRM&id=$1" -O $2
 rm -rf /tmp/cookies.txt
}
```

```shell
gdrive_download 0B4kSFaKe-Rt9dnY2LThVWEZ1dEdjNTQ3UFdTclZmYUtQRFBB intp.stp
```

### Execution
Notice that you must run the command stap as root.
```shell
stap --suppress-handler-errors -g intp.stp ApplicationName
```


### Outputs
Notice that you must run this command as root.  
Receiving Return:

```shell
watch -n2 -d cat /proc/systemtap/stap_*/intestbench
```

IntP returns the interference metrics for CPU, disk, memory, network, and cache. More specifically, it returns the following metrics:

* netp - the percentage of physical network
* nets - the percentage of network queue interference.
* blk - the percentage of disk interference.
* mbw - the percentage of memory bus interference.
* llcmr - the percentage of cache miss.
* llocc - the percentage of cache interference.
* cpu - the percentage of cpu interference.

