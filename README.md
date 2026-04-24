# Linux From Scratch (LFS)

This project details the step-by-step process of building a custom Linux system from scratch, using an existing Linux distribution as the host. The result is a fully functional and bootable Linux system.

## Overview
Linux From Scratch (LFS) is a project that provides users with instructions to build a custom Linux system, completely from source. The process involves creating a minimal base system, compiling necessary tools and packages, and eventually creating a bootable Linux kernel.

## Host System Requirements
To build LFS, your host system must meet the following requirements:
- **CPU**: At least 4 cores
- **Memory**: Minimum 8GB RAM
- **Disk Space**: At least 1GB per core for build space

## Partition Setup
It is recommended to create a dedicated partition for LFS. Using the `fdisk` command, three partitions were created:
- `sda1`: Boot partition
- `sda2`: Swap partition
- `sda3`: Root partition

### Creating a New File System
- **File System Used**: ext4
- Mount the partition to allow the host system to access it.
- Set the `$LFS` variable to manage permissions and isolate the build environment.

## Downloading Packages
- All packages required for the build were downloaded as per the LFS book's package list.
- If the primary source failed, alternative mirrors or individual links were used.

## Building the LFS System
The build process is divided into several phases:

### Phase 1: Creating the Temporary Toolchain
1. **Root User Setup**:
   - Create a limited directory structure as the root user.
   - Compile a cross-compilation toolchain to bootstrap the build process.
   - Transition to an unprivileged `lfs` user for safety.

2. **Unprivileged User Setup**:
   - Create a new bash profile and read the startup files.
   - Use `su -` to enter a clean shell environment.

### Phase 2: Building the Base System
1. **Cross-Compilation Toolchain**:
   - Compile essential utilities for the target system.
   - Install utilities in their final locations.

2. **Enter Chroot Environment**:
   - Mount virtual kernel file systems (`/dev`, `/proc`, `/sys`, `/run`).
   - Transition to a controlled environment, isolating the build process.

3. **Compile Critical Tools**:
   - Build packages like Gettext, Bison, Perl, Python, etc., within the chroot environment.

### Challenges Encountered
- **Circular Dependencies**: Some tools depend on others, requiring careful ordering.
- **Host Interference**: Tools from the host system could interfere with the build process.
- **Command Precision**: Running incorrect commands with root privileges could damage the LFS or host system.

## Bootstrapping the LFS System
1. **Kernel Installation**:
   - Installed Linux kernel version `6.11.8` for hardware interaction and process management.

2. **Configure `/etc/fstab`**:
   - Define how disk partitions and devices are mounted at boot time.

3. **Setup GRUB Bootloader**:
   - Configure GRUB to load the Linux kernel and initialize the system.
   - Ensure correct settings to boot into the LFS environment.

## Rebooting into LFS
1. Exit the chroot environment.
2. Unmount all virtual file systems.
3. Reboot the system into the newly built Linux From Scratch environment.

## Directory Structure
```
LFS/
|-- sources/       # Downloaded source packages
|-- tools/         # Temporary toolchain
|-- root/          # Final root file system
    |-- boot/      # Boot partition
    |-- etc/       # Configuration files
    |-- dev/       # Device files
    |-- proc/      # Process information pseudo-filesystem
    |-- sys/       # System files
    |-- usr/       # User programs
    |-- var/       # Variable data (logs, spools, etc.)
```

## Risks and Precautions
- Always double-check commands when running as `root`.
- Maintain backups of critical data on the host system.
- Follow the LFS book meticulously to avoid issues.


## References
- [Linux From Scratch Book](https://www.linuxfromscratch.org)
