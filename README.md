# Xen-Blanket-NG

Xen-Blanket is a nested-virtualization solution that works without hardware or software support from underlying cloud providers. With Xen-Blanket, you can get hypervisor-level control by running a second-layer Xen hypervisor within virtual machines (VMs) running in clouds. For more information, check out the paper: [The Xen-Blanket: Virtualize Once, Run Everywhere](http://fireless.cs.cornell.edu/publications/xen-blanket.pdf), in EuroSys 2012.

This is a new version of XenBlanket based on Xen 4.13.0 and Linux kernel 4.15.5. Supported underlying (first-layer) hypervisors:

  * Xen
  * KVM
  * VMware ESXi

## Installation

Download the original versions of Linux kernel and Xen, and then apply the patches:

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.15.5.tar.xz
tar xf linux-4.15.5.tar.xz
cd linux-4.15.5
patch -p1 < ../linux-4.15.5.patch

wget https://downloads.xenproject.org/release/xen/4.13.0/xen-4.13.0.tar.gz
tar xf xen-4.13.0.tar.gz
cd xen-4.13.0
patch -p1 <../xen-4.13.0.patch
```

You can now build and install Linux and Xen as normal. Refer to the README files included in the source files. 

### Kernel configuration requirements

For Linux kernel, you need to **enable Xen Dom0 support** in the configuration. This can be done by typing `make xenconfig` before compilation. If you are running on KVM, `make kvmconfig` is useful. A sample `kernel_config` has been provided.

### Grub configuration requirements

For this version of Xen-Blanket, `dom0_vcpus_pin=true` is **required** in the command line options for Xen. You can add the following lines to `/etc/default/grub` (adjust the CPU and memory allocation according to your setup):

```
GRUB_CMDLINE_XEN_DEFAULT="dom0_max_vcpus=4 dom0_mem=4096M,max:4096M dom0_vcpus_pin=true"
GRUB_CMDLINE_LINUX_XEN_REPLACE_DEFAULT="console=hvc0 earlyprintk=xen nomodeset"
```

Make sure that your `/boot` folder contains necessary kernel config file (i.e., ` /boot/config-4.15.5-xennested`) before generating the grub file with `grub2-mkconfig -o /boot/grub2/grub.cfg`.

## Acknowledgements

Xen-Blanket-NG was developed based on the following GPL-licensed code bases:

[1] A version of Xen-Blanket provided by starlab.io: https://github.com/starlab-io/xenblanket-linux

[2] The original Xen-Blanket released by Dan Williams: https://github.com/danlythemanly/xen-blanket

[3] The Supercloud project: https://github.com/zhiming-shen/Xen-Blanket-AGN


