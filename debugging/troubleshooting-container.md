Maybe we sometime complain there is no debugging tool available in container when we debug some issues. Now we should not complain it :)

Here is the procedure how to implement it:

## 1.	Requirement ##
Docker is installed
 
## 2.	Download troubleshooting container image: ##
* docker pull ryanlyy/container-troubleshooting:20180111

You can check the latest version from:

https://hub.docker.com/r/ryanlyy/container-troubleshooting/tags/

## 3.	Debugging on Host layer ##
* docker run --privileged -dt --network=host --ipc=host --pid=host --name tstc ryanlyy/container-troubleshooting:20180111 bash
* docker exec –ti tstc bash
 
Then you can use a lot of tools to debug your issues.

Note: in this case, your container has same network, ipc, pid namespace with host 
 
## 4.	Debugging in Container layer ##

https://github.com/ryanlyy/toolsets/blob/master/scripts/get_container_rootfs.sh

* Get Container Rootfs: ./get_container_rootfs.sh 1df11e5e1068
* docker run --privileged -dt -v /var/lib/docker/aufs/mnt/container-rootfs:/1df11e5e1068 --network=container:1df11e5e1068 --ipc=container:1df11e5e1068 --pid=container:1df11e5e1068 --name tstc ryanlyy/container-troubleshooting:20180111 bash
* docker exec –ti tstc bash

Then you can debug your issue within this container.
 
note: 
*	1df11e5e1068 is the ID of container that you want to debug (docker ps)
*	in this case, your container has same network, ipc, pid namespace and rootfs of the container that you want to debug

## 5. Deploy Container as Pod ##
if you want to deploy this container as pod please refer to pod yaml file as following:
https://github.com/ryanlyy/toolsets/blob/master/troubleshooting-deployment.yaml

## 6.	Tools supported so far ##
Tcpdump, gdb, ip, ethtool, netstat, arping, strace, iptraf-ng, jnettop, iftop, iotop, bmon, traceroute, vmstat, vnstat, openssh, htop, sctp_darn, lsof, iostat, collectl, mpstat, socat, mytop, apachetop, iperf, dig, qperf, etc.
 
With those tools, you can check your networking, cpu, disk, memory, process etc. issues.
 
The following is packages installed:

* Networking:
iproute2 iputils net-tools jnettop iftop tcpdump ethtool nethogs iptraf-ng bmon traceroute iptstate  vnstat nmap nmon smokeping nmap-ncat wget lksctp-tools socat vnstat iperf bind-utils qperf
* Other:
ngrep mrtg ngios openssh strace unzip python-pip htop gdb openssh-server yajl-devel libcurl-devel which  sysstat htop atop apachetop mytop iotop dstat mpstat pmap collectl iostat lsof
