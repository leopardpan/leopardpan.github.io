---
layout: post
title: "bash脚本测试Ubuntu服务器的性能并输出报告"
date: 2023-01-17
description: "bash脚本测试Ubuntu服务器的性能并输出报告"
tag: Linux
---  

bash脚本测试Ubuntu服务器的性能并输出报告

```bash

#!/bin/bash

# This script is used to test server performance and output report

# Get the system information
hostname=`hostname`
kernel=`uname -r`

# Get the CPU information
cpu_model=`cat /proc/cpuinfo | grep "model name" | head -1 | cut -d: -f2`
cpu_cores=`cat /proc/cpuinfo | grep "cpu cores" | head -1 | cut -d: -f2`
cpu_freq=`cat /proc/cpuinfo | grep "cpu MHz" | head -1 | cut -d: -f2`

# Get the memory information
mem_total=`cat /proc/meminfo | grep "MemTotal" | cut -d: -f2`
swap_total=`cat /proc/meminfo | grep "SwapTotal" | cut -d: -f2`

# Get the disk information
disk_total=`df -h | grep "/$" | awk '{print $2}'`
disk_used=`df -h | grep "/$" | awk '{print $3}'`

# Print the report
echo "System Information"
echo "-----------------"
echo "Hostname: $hostname"
echo "Kernel: $kernel"
echo ""
echo "CPU Information"
echo "---------------"
echo "Model: $cpu_model"
echo "Cores: $cpu_cores"
echo "Frequency: $cpu_freq MHz"
echo ""
echo "Memory Information"
echo "-----------------"
echo "Total: $mem_total"
echo "Swap: $swap_total"
echo ""
echo "Disk Information"
echo "----------------"
echo "Total: $disk_total"
echo "Used: $disk_used"
echo ""
```

结果如下：
```console
root@hugo-virtual-machine:/opt# sh service.sh 
System Information
-----------------
Hostname: hugo-virtual-machine
Kernel: 5.15.0-58-generic

CPU Information
---------------
Model:  AMD Ryzen 7 4800H with Radeon Graphics
Cores:  2
Frequency:  2894.461 MHz

Memory Information
-----------------
Total:         8105812 kB
Swap:        1999868 kB

Disk Information
----------------
Total: 23G
Used: 12G
```