## Introduction
This guide provides the steps to replace the RXE code in the Linux kernel with your custom RXE code and compile the kernel. We will use the Linux kernel version 5.4.127 for this process. The kernel source can be downloaded from the official Linux Kernel website.

## RXE Module Overview
This repository contains the core implementation of both ACC and DCQCN scheme, as presented in the paper "Revisiting Congestion Control for Lossless Ethernet" by Y Zhang et al.

## Prerequisites
Before starting, ensure you have the following tools installed:

* gcc (GNU Compiler Collection)
* make (Build automation tool)
* curl or wget (to download the kernel)
* Development libraries like libncurses-dev, libssl-dev, etc.

## Step 1: Download the Linux Kernel Source
Start by downloading the kernel source code for version 5.4.127. Use the following command:

```
wget https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.4.127.tar.gz
```
Alternatively, you can use curl:

```
curl -O https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.4.127.tar.gz
```
Next, extract the downloaded tarball:

```
tar -xvf linux-5.4.127.tar.gz
cd linux-5.4.127
```

## Step 2: Replace RXE Code in the Kernel
To replace the RXE code, follow these steps:

Navigate to the directory where the RXE code is located in the kernel source. The default path for RXE code in the kernel is:

```
cd /source/drivers/infiniband/sw/rxe
```
Backup the existing RXE code (in case you need to revert changes):

```
cp -r /source/drivers/infiniband/sw/rxe /source/drivers/infiniband/sw/rxe_backup
```
Replace the RXE code with your custom RXE code. For example, if your custom RXE code is stored in /path/to/your/rxe, you can copy the new code over the existing one:

```
cp -r /path/to/your/rxe/* /source/drivers/infiniband/sw/rxe/
```
Ensure all the necessary files are replaced in this directory.

## Step 3: Configure the Kernel
Before compiling the kernel, you must configure it. You can use the default configuration or create a custom one.

To configure the kernel, use the following command:

```
make menuconfig
```
This will open a menu where you can enable or disable kernel features. Make sure that the InfiniBand/RXE support is enabled.

Alternatively, you can copy an existing configuration file from your current kernel:

```
cp /boot/config-$(uname -r) .config
```
Update the configuration:

```
make oldconfig
```
## Step 4: Compile the Kernel
Once the kernel is configured, proceed to compile it. Run the following commands:

Clean up any previous build artifacts (optional):

```
make clean
```
Compile the kernel using all available CPU cores:

```
make -j$(nproc)
```
The nproc command will automatically use all available CPU cores to speed up the compilation process.

Optionally, compile the kernel modules:

```
make modules
```

Step 5: Install the Kernel
After the kernel is compiled, install it and its modules:

Install the modules:

```
sudo make modules_install
```
Install the kernel:

```
sudo make install
```
This will install the kernel and modules to /boot and /lib/modules/.

Update the bootloader (GRUB or other):

For GRUB, you can run:

```
sudo update-grub
```
Reboot the system:

```
sudo reboot
```
After rebooting, your custom kernel with the new RXE code will be active.

Step 6: Verify the Installation
After rebooting, verify that the kernel has been installed successfully:

```
uname -r
```
This should show 5.4.127 (or the custom version you built).

Additionally, check if the RXE module is loaded:

```
lsmod | grep rxe
```
If the RXE module is loaded, it will be listed in the output.

## Conclusion
You have successfully replaced the RXE code in the Linux kernel with your custom RXE code and compiled the kernel. This version includes the ACC and DCQCN congestion control schemes for efficient lossless Ethernet operation, as described in the paper by Y Zhang et al. If you encounter any issues, check the kernel logs using dmesg for troubleshooting.