# Setting up a Jailbroken iPhone with SSH Access


## Introduction

Once you have jailbroken the iOS device, you will need to connect to the device over SSH. At this point, I normally switch to a Kali Linux virtual machine and connect the device to the VM using the `Devices > USB > <iOS device>` option within Virtual Box (this will be different for VMware). Alternatively, you can just use the bare-metal Ubuntu Linux install from the earier steps. This is sole preference. There are multiple ways to set up SSH on an iOS device. I personally prefer connecting over USB using `iproxy` and then remote port forwarding port `8080` so traffic can then be intercepted with burp suite.


## Installing Required Tools/Libraries

Install the following tools/libraries using `apt` on either Ubuntu Linux or Kali Linux:

`sudo apt install libusbmuxd6 libusbmuxd-dev libusbmuxd-tools libimobiledevice6 libimobiledevice-dev libimobiledevice-utils -y`

## On the iOS device

Open Cydia and install `OpenSSH` it is also recommended to install "Frida". Add the repository in Cydia:

`https://build.frida.re`

Refresh the repositories and search for "Frida". Once found, install the latest version.

## Setup SSH connection from Linux to iOS device

You will need multiple terminal windows open to do this.

1) In the first terminal type `iproxy 2222 44`. This will create a local port "2222" that can be accessed over USB and will redirect to the port 44 that the iPhone has set up for ssh.

2) In a second terminal type `ssh root@127.0.0.1 -p 2222 -R 8080:127.0.0.1:8080`. The when connecting to SSH the default username is `root` and the default password is `alpine.`

The SSH command will connect to the device over USB using local port "2222" which is redirected to port "44" on the iPhone (iproxy command set this up)

The `-R 8080:127.0.0.1:8080` part of the SSH command remote port forwards 8080 onto the iOS device. 
*In simple terms, I want to put my local port 8080 on Linux onto the iPhone as port 8080*

The iPhone can then be configured to forward traffic to port 8080 by setting up a "proxy" within the WiFi settings.

1) Go to Settings > WiFi
2) Click WiFi network
3) Go to proxy
4) Click "Manual"
5) Set host to 127.0.0.1
6) Set port to 8080

This will mean that all internet traffic will be forwarded to port 8080. Because of the SSH remote port forward, this will then be routed to port "8080" on the Linux machine. 

Once Burp Suite is open, this listens, by default, for traffic on port "8080". Therefore this will put you in a position to be able to intercept traffic.

