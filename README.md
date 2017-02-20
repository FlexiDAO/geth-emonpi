# geth-emonpi
Building a Geth node on an emonPi (Metering device based on Raspberry Pi 3 from OpenEnergyMonitor). 

This tutorial aims to install the latest Geth version available at the time of writing (Ethereum Geth v1.5.8) on an EmonPi metering device (https://guide.openenergymonitor.org/setup/). The emonPi is based on A Raspberry Pi 3, therefore this tutorial can be used to install Geth on a RPi 3 with few minor changes.

The emonPi comes wit EmonSD, a 8GB pre-built SD card for Raspberry Pi running as an emonPi / emonBase based on the RASPBIAN JESSIE LITE. 
In this tutorial we will use geth testnet light client so 8GB SD might be enough, however before starting (especially if you want to use the full node) it's always better to check the current status of the blockchain and consider the future growth. 
If you want to use a bigger SD you can flash the card following these instructions: https://github.com/openenergymonitor/emonpi/wiki/emonSD-pre-built-SD-card-Download-&-Change-Log

Before starting, you will need to make the filesytem RW with $rpi-rw before installing any packages then back to RO with $rpi-ro when you're done. (not for RPi3)

INSTALLING DEPENDENCIES

In order to install Geth we need to install a few tools and software packages. First let's make sure everything is up to date and update it if it isn't:

```
pi@emonpi(rw):~$ sudo apt-get update
pi@emonpi(rw):~$ sudo apt-get upgrade -y
```

The second command might take a while if you have installed the light Jessie os.

We need to install some dependencies:
```
pi@emonpi(rw):~$ sudo apt-get install libgmp3-dev -y
```
Next we need to download and install the latest version of the Go programming language (Go 1.7.3). Older versions of Go will not support the latest version of Geth. EmonSD has 3 partitions, we will work in ~/data.
```
pi@emonpi(rw):data$ mkdir bin
pi@emonpi(rw):data$ cd bin
pi@emonpi(rw):bin$ wget https://storage.googleapis.com/golang/go1.7.3.linux-armv6l.tar.gz  //To download the arm binary file
pi@emonpi(rw):bin$ tar -xzvf go1.7.3.linux-armv6l.tar.gz
pi@emonpi(rw):bin$ export GOROOT=home/pi/data/bin/go
pi@emonpi(rw):bin$ export PATH=$PATH:$GOROOT/bin
```
Now run the command $go version to check if the installation was succesful.

INSTALLING GETH

Download the latest Geth source code from github and build it 
```
pi@emonpi(rw):bin$ git clone -b release/1.3.3 https://github.com/ethereum/go-ethereum.git
pi@emonpi(rw):bin$ cd go-ethereum/
pi@emonpi(rw):go-ethereum$ make geth
pi@emonpi(rw):go-ethereum$ sudo cp build/bin/geth /usr/local/bin/
```
If, during the installation, you get an error "no space left on device", follow these steps and the rune the command make-geth again:
```
pi@emonpi(rw):data$ mkdir newtmp
pi@emonpi(rw):~$ sudo mount -B /home/pi/data/newtmp /tmp
```
In order to store the database in the ~/data partition, we need first to create the directory ethData and the run geth with the flag --datadir /home/pi/data/ethData
```
pi@emonpi(rw):data$ mkdir ~/ethData
```
You can now start the node on your emonPi:
```
pi@emonpi(ro):~$ geth --testnet --light --datadir /home/pi/data/ethData console
```
