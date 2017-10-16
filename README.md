# Setting up the Deep Learning Environment

### My system specifications
* Operating System: Elementary OS Loki 0.4.1
* Graphics Card: NVIDIA GeForce GTX 1050 Ti

### Table of Contents
* [Basic OS updates](#basic-os-updates)
* [NVIDIA Drivers](#nvidia-drivers)
* [CUDA Toolkit](#cuda-toolkit)
* [cuDNN](#cudnn)
* [Tensorflow](#tensorflow)

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
* Blacklist Nouveau kernel driver. Open `/etc/modprobe.d/blacklist.conf` and add the following:

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
        sudo apt-get install cuda-8-0
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

**Note:** `-j $(($(nproc) + 1))` executes the make command in parallel using the number of cores in the machine, so the compilation is faster.

* Run deviceQuery and ensure that it detects the graphics card and the tests pass

        bin/x86_64/linux/release/deviceQuery
* Remove 'cuda-samples' by `rm -r ~/cuda-samples`

### cuDNN
* cuDNN is a GPU accelerated library for DNNs. It can help speed up execution in many cases.
* To download the cuDNN library, register/login at [NVIDIA cuDNN](https://developer.nvidia.com/cudnn)
* Download 'cuDNN v7.0 Library for Linux'. Here are the instructions for installing from a tar file:

        tar -xzvf cudnn-8.0-linux-x64-v6.0.tgz
        sudo cp cuda/include/cudnn.h /usr/local/cuda/include
        sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
        sudo chmod a+r /usr/local/cuda/include/cudnn.h
* Verify cuDNN

        cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

### Tensorflow
* NVIDIA requirements to run TensorFlow with GPU support:
    * CUDA® Toolkit 8.0. Ensure that you append the relevant Cuda pathnames to the `LD_LIBRARY_PATH` environment variable as described in the NVIDIA documentation.
    * The NVIDIA drivers associated with CUDA Toolkit 8.0.
    * cuDNN v6. Ensure that you create the `CUDA_HOME` environment variable as described in the NVIDIA documentation.
    * GPU card with CUDA Compute Capability 3.0 or higher. See [NVIDIA documentation](https://developer.nvidia.com/cuda-gpus) for a list of supported GPU cards.
    * The `libcupti-dev` library, which is the NVIDIA CUDA Profile Tools Interface. This library provides advanced profiling support. To install this library, issue the following command:

            sudo apt-get install libcupti-dev
* Installing with virtualenv
    * Install pip and virtualenv by issuing the following command:

            sudo apt-get install python3-pip python3-dev python-virtualenv # for Python 3.n
    * Create a virtualenv environment by issuing the following command:

            mkdir ~/tensorflow
            virtualenv --system-site-packages -p python3 ~/tensorflow # for Python 3.n
     * Activate the virtualenv environment by issuing the following command:

            source ~/tensorflow/bin/activate # bash, sh, ksh, or zsh
          The preceding source command should change your prompt to the following:

            (tensorflow)$
    * Ensure pip ≥8.1 is installed:

            (tensorflow)$ easy_install -U pip
    * Issue the following command to install TensorFlow in the active virtualenv environment:

            (tensorflow)$ pip3 install --upgrade tensorflow-gpu # for Python 3.n and GPU
* Validate installation:
    * Invoke python from your shell and Enter the following short program inside the python interactive shell:

            # Python
            import tensorflow as tf
            hello = tf.constant('Hello, TensorFlow!')
            sess = tf.Session()
            print(sess.run(hello))
        If the system outputs the following, then you are ready to begin writing TensorFlow programs:

            Hello, TensorFlow!
