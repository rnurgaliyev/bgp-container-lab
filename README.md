# BGP Lab for SREs – Self-Study Guide

Welcome to the **BGP Lab for SREs**! This lab is designed to help you develop a solid understanding of BGP and networking fundamentals and troubleshooting techniques, enabling you to effectively manage and optimize network routing in large-scale environments.

## Lab Overview

This lab includes:
1. **Containerlab Topology**: A pre-configured lab setup to emulate simple real-world networking scenarios. Topology diagram is available in [PDF](topology.pdf) or [SVG](topology.svg) formats.
2. **Configuration Tasks**: Hands-on exercises to set up and optimize networking.
3. **Troubleshooting Tasks**: Practical challenges to diagnose and resolve common networking issues.

All scenarios in this lab are vendor-neutral. They focus on common BGP setups, ACLs, and firewall rules, without using vendor-specific features or forwarding settings.

## Requirements

To complete this lab, you'll need:
- **Containerlab** installed on your machine. Follow [this installation guide](https://containerlab.dev/). For MacOS users, running Containerlab via Docker Desktop might present issues. It's recommended to utilize a straightforward Linux virtual machine with Docker installed. You can follow [these instructions](VM.md) to set up a VM using Qemu and install Containerlab within it.
- A text editor for modifying configuration files. If you are using the VM, it is recommended to choose an editor that can connect to a remote machine (e.g., VSCode, Zed).

## Getting Started

1. **Clone the repository**:
    ```bash
    git clone https://github.com/rnurgaliyev/bgp-container-lab
    cd bgp-container-lab
    ```

2. **Deploy the lab topology**:
    ```bash
    sudo containerlab deploy
    ```

3. **Access lab devices**:
    If lab topology starts successfully, you can list docker containers and run linux or BIRD commands normally
    ```
    lab@lab:~/bgp-container-lab$ docker ps
    CONTAINER ID   IMAGE           COMMAND     CREATED          STATUS          PORTS     NAMES
    370cf90ffdd2   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-eyeball01.tra02
    b695f0468010   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-eyeball01.tra03
    8c09926b0cbc   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router01.lab02
    ecc21ed0b7e1   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-server02.lab01
    13776f5dae59   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-server02.lab02
    b77f056ce4e8   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-server01.lab02
    5de9390c4670   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router02.tra02
    76cb51a98478   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-eyeball01.tra01
    5b5364bc4032   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router01.tra02
    1e0b219c00a8   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-server03.lab02
    4407c5bf0168   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router03.tra02
    5b8b07b9dfd9   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router01.tra01
    a2eb2efb7ffd   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-server01.lab01
    f58c022a7358   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-server03.lab01
    641b8f276203   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router01.lab01
    139e49bf2c8e   alpine:latest   "/bin/sh"   35 minutes ago   Up 35 minutes             clab-bgp-lab-router01.tra03

    lab@lab:~/bgp-container-lab$ docker exec clab-bgp-lab-router01.tra01 ip -br a
    lo               UNKNOWN        127.0.0.1/8 10.101.254.1/32 fc00:b:101::254:1/128 ::1/128
    eth0@if244       UP             172.20.20.7/24 3fff:172:20:20::7/64 fe80::42:acff:fe14:1407/64
    eth2@if268       UP             10.102.101.2/30 fc00:b:102::101:2/126 fe80::a8c1:abff:fe66:fd3b/64
    eth1@if275       UP             10.101.0.1/30 fc00:b:101::1/126 fe80::a8c1:abff:fec5:8a3d/64
    eth3@if293       UP             192.168.1.1/24 fc00:b:101::beef:1/64 fe80::a8c1:abff:fe9a:738e/64

    lab@lab:~/bgp-container-lab$ docker exec clab-bgp-lab-router01.tra01 birdc show protocols
    BIRD 2.15.1 ready.
    Name       Proto      Table      State  Since         Info
    device1    Device     ---        up     14:39:29.182
    direct1    Direct     ---        up     14:39:29.182
    kernelv4   Kernel     master4    up     14:39:29.182
    kernelv6   Kernel     master6    up     14:39:29.182
    lab01      BGP        ---        up     15:15:51.814  Established
    transit02  BGP        ---        up     14:39:32.990  Established
    ```

## Lab Tasks

See [TASKS](TASKS.md).
