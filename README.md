# Setting up the Deep Learning Environment

### My system specifications
* Operating System: Elementary OS Loki 0.4.1
* CPU: 
* Graphics Card: NVIDIA GeForce GTX 1050 Ti

### Table of Contents
* [Basic OS updates](#basic-os-updates)
* [Nvidia Drivers](#nvidia-drivers)
* [CUDA Toolkit](cuda-toolkit)

### Basic OS updates
* Making OS up-to-date:

        sudo apt-get update
        sudo apt-get upgrade
        sudo apt-get install build-essential cmake g++ gfortran git pkg-config python-dev software-properties-common wget
        sudo apt-get autoremove
        sudo rm -rf /var/lib/apt/lists/*

### Nvidia Drivers
* Installing the recommended driver from 'Proprietary GPU Drivers' PPA repository

        sudo add-apt-repository ppa:graphics-drivers/ppa
        sudo apt-get update
* Blacklist Nouveau kernel driver
Open `/etc/modprobe.d/blacklist.conf` and add the following:

        blacklist amd76x_edac
        blacklist vga16fb
        blacklist nouveau
        blacklist rivafb
        blacklist nvidiafb
        blacklist rivatv
* Check for the recommended driver

        sudo ubuntu-drivers devices
* Install the recommended driver

        sudo apt-get install nvidia-387
* Restart system

	sudo shutdown -r now
* Ensure that the correct version of NVIDIA driver is installed

	cat /proc/driver/nvidia/version
* Do a check to ensure everything is good so far using the `nvidia-smi` command. This should output some stats about the GPU.

### CUDA Toolkit
* Download CUDA Toolkit from [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) and follow the instructions provided there.

	sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
	sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
	sudo apt-get update
	sudo apt-get install cuda
* Add CUDA to the environment variables

	echo 'export CUDA_HOME=/usr/local/cuda' >> ~/.bashrc
	echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
	echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
	source ~/.bashrc
* Ensure the correct version of CUDA is installed

	nvcc -V
	cat /usr/local/cuda/version.txt
* Restart system

	sudo shutdown -r now
* Verifying CUDA Toolkit functioning

	/usr/local/cuda/bin/cuda-install-samples-9.0.sh ~/cuda-samples
	cd ~/cuda-samples/NVIDIA*Samples
	make -j $(($(nproc) + 1))

**Note: **`-j $(($(nproc) + 1))` executes the make command in parallel using the number of cores in your machine, so the compilation is faster.

* Run deviceQuery and ensure that it detects the graphics card and the tests pass

	bin/x86_64/linux/release/deviceQuery
* Remove 'cuda-samples' by `rm -r ~/cuda-samples`
