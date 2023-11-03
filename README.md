# Docker-Limiter-Desktop

Docker-Limiter-Desktop is a dedicated system utility built to seamlessly manage system resources allocated to Docker daemon on desktop environments.

## Description

Docker-Limiter-Desktop exploits shell scripts in conjunction with `systemd` slice and cgroups to deftly adjust the CPU and Memory quotas and limits allocated to all Docker containers. The configurations for `systemd` slices are dynamically altered based on available system resources, ensuring an equilibrium in system performance while averting potential resource exhaustion scenarios. Additionally, a main configuration file is available to manage the daemon and apply user-specific preferences and settings.

## Features

- **Dynamic Memory Management:** Adjusts memory limit for Docker containers by dynamically altering `systemd` slice configurations based on available system memory. **Default allocate 90% of memory.**

- **CPU Optimization:** Intelligently modifies the CPU quota for Docker containers, adapting to the available CPU cores and their utilization. **Default allocate 90% of CPUs.**

- **Daemon Configuration:** Provides a main configuration file to manage the daemon and its operational parameters.
## How to Use

### Prerequisites

- Docker installed and configured
- systemd for managing services and slices
- Root permissions for managing systemd services and configurations
  
### Installation and Setup
Download the deb release and  
 ```sh
sudo apt install ./docker-limiter-desktop.deb
 ```


## Configuration

### System Resource Limits

Modify the system resource limitations applied to Docker containers by editing the configuration file located at `/etc/default/docker-limiter-desktop`. 

Here is a basic guide on how to configure the resource limitations:

```plaintext
# /etc/default/docker-limiter-desktop
# Configuration file for Docker Limiter Desktop

# Short description:
# MAX_MEMORY: Maximum percentage of total system memory allocated for Docker containers
# MAX_CPU: Maximum percentage of total system CPU allocated for Docker containers

# The Docker daemon will be automatically restarted after modifications to this file.

```

### Doc
See the cgroups resources usage
```
root@host:/root# systemd-cgtop
Control Group                                                                                                                       Tasks   %CPU   Memory  Input/s Output/s
/                                                                                                                                    1279  119.6     2.7G   178.1K   120.7K
docker.slice                                                                                                                          204  101.3   852.3M        -        -
docker.slice/docker-limiter.slice                                                                                                     204  101.3   852.2M        -        -
docker.slice/docker-limiter.slice/docker-limiter-desktop.slice                                                                        204  101.3   852.2M        -        -
```
See the heritage
```
root@host:/root#  systemd-cgls -u docker-limiter-desktop.slice
Unit docker-limiter-desktop.slice (/docker.slice/docker-limiter.slice/docker-limiter-desktop.slice):
├─docker-8af0064166bd382691f20e43eb5613338c6126e914da30c87a38ff7884cf6130.scope …
│ ├─38491 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38539 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38540 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38541 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38542 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38543 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38544 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38545 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38546 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38547 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38548 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38549 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38550 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38551 stress --cpu 100 --vm 1 --vm-bytes 2G
│ ├─38552 stress --cpu 100 --vm 1 --vm-bytes 2G
...
```

# Dev / test
Build manually the package 
```
dpkg-deb --build docker-limiter-desktop
```
