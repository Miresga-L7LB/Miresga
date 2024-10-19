# Miresga

## 1. Introduction
This repository hosts Miresga, a hybrid and high-performance layer-7 load balancing system.


## 2. Requirement
### I. Programmable Switch
We have only fully tested our prototype on SDE 9.4.0. While our data plane was successfully compiled on a virtual machine with SDE 9.13.1, the API for the control plane seems to be inconsistent. Therefore, using a higher version may require some modifications to the code.

### II. Front-end Server
We tested on Ubuntu 20.04 with the 5.4.0-196-generic kernel and DPDK-19.11. Using a more recent version of DPDK may also require updating some APIs in the code. Additionally, the following are required to build the project: **cmake**, **meson**, and **ninja**. You can install them by running:
```
sudo apt install build-essential meson ninja cmake
```

### III. Back-end Agent
To run the backend agent, you may need to install the following:
```
sudo apt install clang llvm libelf1 libelf-dev libbpf-dev zlib1g-dev
```

The versions of clang and llvm must be higher than 11.0.0. Therefore, for Ubuntu 20.04, you may need to install specific versions:

```
sudo apt install clang-11 llvm-11
```
and then modify the Makefile in the Backend directory.

We have only tested the XDP native mode on Mellanox CX6 NICs and have not tested with other NICs, so additional modifications may be required for other cards.

## 3. Quick Start
First, clone the repository:
```
git clone --recurse-submodules https://github.com/Miresga-L7LB/Miresga.git 
git submodule init
```
### I. Programmable Switch
#### A. Data plane
On SDE 9.4.0, we used the following code to compile:
```
cd $SDE
./p4_build.sh /path/to/Miresga/Switch/data_plane/l7lb_switch.p4
```
#### B. Control plane
```
cd /path/to/Miresga/Switch/control_plane
make
```
#### C. Running
```
cd /path/to/Miresga/Switch/control_plane
./l7lb_control_plane
```

### II. Front-end Server
```
cd /path/to/Miresga/Frontend
meson build
ninja -C build
cd build
sudo su
./L7LB_Server
```

### III. Back-end Server
```
cd /path/to/Miresga/Backend
make
cd build
sudo su
./xdp_loader ${iface_name}
```
You need to replace `${iface_name}` with the actual network interface name.
If you want to unload the eBPF program, execute:
```
cd /path/to/Miresga/Backend
sudo su
sh unload.sh ${iface_name}
```
