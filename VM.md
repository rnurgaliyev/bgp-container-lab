## Preparing a virtual machine for Containerlab

This guide is designed for MacOS users with ARM (M1 and further) hardware. If you have an older Intel Mac, you will need to modify this guide yourself, but it is straightforward.

This guide sets up a virtual machine with 8GiB of RAM and a 10GiB virtual hard drive. This configuration is sufficient to run all scenarios in this lab. However, if resources are limited, feel free to adjust it.

1. **Install Qemu**:

    There are many way to do it, simplest is just to use Brew:

    ```bash
    brew install qemu
    ```

2. **Make a directory for the lab VM**:

    We will require multiple files, so it's best to keep things organized.

    ```bash
    mkdir bgp-lab-vm
    cd bgp-lab-vm
    ```

3. **Download and prepare Debian image and BIOS image**:

    ```bash
    # Update URLs if needed
    DEBIAN_IMAGE_URL=https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-nocloud-arm64.qcow2
    DEBIAN_IMAGE_PATH=./disk.qcow2

    BIOS_IMAGE_URL=http://ftp.de.debian.org/debian/pool/main/e/edk2/qemu-efi-aarch64_2024.11-2_all.deb
    BIOS_IMAGE_PATH=./bios.deb

    # Download Debian and BIOS images
    curl -L "$DEBIAN_IMAGE_URL" -o "$DEBIAN_IMAGE_PATH"
    curl -L "$BIOS_IMAGE_URL" -o "$BIOS_IMAGE_PATH"

    # Resize Debian image
    qemu-img resize "$DEBIAN_IMAGE_PATH" 10G

    # Extract BIOS
    tar xOf "$BIOS_IMAGE_PATH" data.tar.xz | tar xJO usr/share/qemu-efi-aarch64/QEMU_EFI.fd > QEMU_EFI.fd

    # Remove BIOS deb archive
    rm "$BIOS_IMAGE_PATH"
    ```

4. **Start the virtual machine**:

    To start the virtual machine, save this script as a .sh file in the same directory, and modify it if needed. Typically no changes are required besides `NIC_HOST_INTERFACE`, which should be a host interface with DHCP and Internet access. Normally it is your laptops WiFi interface. The virtual machine will be bridged to this interface and will obtain its own IP address.

    ```bash

    NIC_MAC=52:54:00:ff:01:01
    NIC_HOST_INTERFACE=en0

    sudo qemu-system-aarch64 \
        -machine virt,accel=hvf,highmem=on \
        -bios ./QEMU_EFI.fd \
        -cpu host \
        -smp 2 \
        -m 8G \
        -drive if=virtio,format=qcow2,file=./disk.qcow2 \
        -nic vmnet-bridged,mac=$NIC_MAC,ifname=$NIC_HOST_INTERFACE \
        -nographic
    ```

    Running this script launches the VM in the current shell, eliminating the need for a GUI or separate console window. You can reuse this script to return to the VM after a pause, as there is no state to maintain apart from the disk image. Exit the VM and terminate Qemu with the Ctrl+A X key sequence.

5. **Do the minimal configuration**:

    After booting the VM, login as `root` and prepare it to run Containerlab.

    ```bash
    echo lab > /etc/hostname
    hostname lab

    apt update
    apt -y install cloud-guest-utils

    # Expand root partition
    growpart /dev/vda 1
    resize2fs /dev/vda1

    # Install necessary software
    apt -y install ca-certificates curl openssh-server git

    # Set up a user and group for SSH access
    groupadd lab
    useradd -g lab -G sudo -m -d /home/lab -s /bin/bash lab

    # Disable password for sudo
    echo "%sudo ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/nopasswd

    # Replace with your own SSH key
    mkdir /home/lab/.ssh
    echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICv3PmX0+KZyDqrNmMIsO3WWrLWqomHLSXwleRdCaw8G" > /home/lab/.ssh/authorized_keys
    chown -R lab:lab /home/lab/.ssh

    # Note down virtual machine IP address (for SSH)
    ip -br a
    ```

6. **Switch to SSH and proceed with configuration**:

    Connect to the virtual machine via SSH using the `lab` user and run following script.

    ```bash
    # Install Docker
    echo "deb [trusted=yes] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list

    sudo apt update && sudo apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    # Add user to the Docker group
    sudo usermod -a -G docker lab
    newgrp docker

    # Install Containerlab (ignore APT errors, it's a lab VM anyways)
    echo "deb [trusted=yes] https://netdevops.fury.site/apt /" | sudo tee /etc/apt/sources.list.d/netdevops.list
    sudo apt update && sudo apt -y install containerlab
    ```

7. Your VM is now fully prepared to run Containerlab! Proceed with instructions in [README](README.md#getting-started).
