# Miresga

## 1. Introduction
This repository hosts Miresga, a hybrid and high-performance layer-7 load balancing system.

## 2. Requirement
### I. Programmable Switch
We have only tested on a Tofino switch with SDE-9.4.0, so there may be some issues when using a Tofino switch with a higher SDE version.

### II. Front-end Server
We tested on Ubuntu 20.04 with the 5.4.0-196-generic kernel and DPDK-19.11. Using a more recent version of DPDK may require updating some APIs in the code. Additionally, the following are required to build the project: **cmake**, **meson**, and **ninja**. You can install them by running:
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
