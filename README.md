# Setting up the Deep Learning Environment

### My system specifications
* Operating System: Elementary OS Loki 0.4.1
* CPU: 
* Graphics Card: NVIDIA GeForce GTX 1050 Ti

## Table of Contents
1. [Basic OS updates](#Basic OS updates)
2. [Nvidia Drivers](#Nvidia Drivers)

### Basic OS updates
* Run the following commands to make your OS up-to-date:

  `sudo apt-get update`
  `sudo apt-get upgrade`
  `sudo apt-get install build-essential cmake g++ gfortran git pkg-config python-dev software-properties-common wget`
  `sudo apt-get autoremove`
  `sudo rm -rf /var/lib/apt/lists/*`

### Nvidia Drivers
* We'll be installing the recommended driver from 'Proprietary GPU Drivers' PPA repository

  `sudo add-apt-repository ppa:graphics-drivers/ppa`
  `sudo apt-get update`
* Blacklist Nouveau kernel driver
Open `/etc/modprobe.d/blacklist.conf` and add the following:
  `blacklist amd76x_edac`
  `blacklist vga16fb`
  `blacklist nouveau`
  `blacklist rivafb`
  `blacklist nvidiafb`
  `blacklist rivatv`
* Check for the recommended driver
  `sudo ubuntu-drivers devices`
* Install the recommended driver
  `sudo apt-get install nvidia-387`
