---
layout:     post
published: false
title:      "Cloudera Installation - Easy way for big data approach"
subtitle:   "How to install Cloudera CDH5 on Centos 7"
date:       2016-08-01
author:     "Canh Tran"
header-img: "inblog/20160831/xdata.png"
tags:
    - Cloudera
    - CDH
    - Big Data
    - Hadoop
    - Centos7
---

#### Senarios:
I have 5 machines which will be divided into one master and four nodes. Below is the specs of each machine:

>
| Name Node (Dell R730)                        | Data nodes (Dell R730xd)              |
-------------------------------------------------| ----------------------------------------
| - 2 x  E5-2630 v3 2.4GHz 8 Cores          | - 2 x  E5-2630 v3 2.4GHz 8 Cores
| - 64GB RAM                                | - 32GB RAM
| - 2 x 300GB 15k SAS 2.5" HDD              | - 2 x 300GB 15k SAS 2.5" HDD (flex bay)
| - 8 x 1TB 7.2k NL-SAS 2.5" HDD            | - 8 x 1TB 7.2k NL-SAS 2.5" HDD
| - PERC H730                               | - PERC H730
| - 1x Intel x520 Dual Port 10Gb SFP+ + I350 Dual Port 1Gb Network Daughter Card                              | - 1x Intel x520 Dual Port 10Gb SFP+ + I350 Dual Port 1Gb Network Daughter Card
|                              | - 2x 10Gbe SR Optic Transceivers

&nbsp;

> **Note:** All machines are running `Centos 7`

#### `Let's start`

Make sure all have root password. If not, here is how to set root password

```bash
$ su -i
$ passwd
```

&nbsp;


#### <i class="fa fa-server" aria-hidden="true"></i> For all Machines   


- Turn off **firewall**

```bash
$ service iptables stop
$ chkconfig iptables off
$ service firewalld stop
$ chkconfig firewall off
```

- **IPv6 Support**

```bash
$ sysctl -w net.ipv6.conf.default.disable_ipv6=1
$ sysctl -w net.ipv6.conf.all.disable_ipv6=1
```

- Change **swappiness**

```bash
$ sudo vi /etc/sysctl.conf
Add line: vm.swappiness=10
$ sudo sysctl vm.swappiness=10
```

- Turn off **Huge defrag**

```bash
$ echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

- Install **Network Transfer Protocal**

```bash
$ sudo yum install ntp -y
$ sudo service ntpd start
$ chkconfig ntpd on
```
- Disable **Selinux**

```bash
$ vi /etc/sysconfig/selinux
Changing: SELINUX=disabled
Restart centos
$ shutdown -r now
```

&nbsp;

#### <i class="fa fa-desktop" aria-hidden="true"></i> Master node machine
- Install **wget**

```bash
$ sudo yum install wget -y
```
- Install **Java**

Since Cloudera CDH won't support OpenJdk, we need to install Oracle JDK

```bash
$ cd ~
$ wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jre-8u60-linux-x64.rpm"
$ sudo yum localinstall jre-8u60-linux-x64.rpm
```

&nbsp;

#### <i class="fa fa-download" aria-hidden="true"></i> Cloudera Installation

**Step 1 :** Download cloudera manager

```bash
$ wget https://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
```

**Step 2 :** Change permission of cloudera-manager-installer.bin file

```bash
$ chmod u+x cloudera-manager-installer.bin
```

**Step 3 :** Execute the file

```bash
$ ./cloudera-manager-installer.bin
```

**Step 4 :** Accept the term and agreement and waiting for it download.
Read the agreement and choose Return or press Enter to choose Next

**Step 5 :**
- Open `http://localhost:7180` to go to cloudera manager
- Username and Password for Cloudera manager is **admin/admin**

*Follow the instruction from the website to download and install CDH together with Hbase, Spark, Hive.. packages*
