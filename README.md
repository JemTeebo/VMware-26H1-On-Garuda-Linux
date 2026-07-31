# VMware-26H1-On-Garuda-Linux

I decided to put my first guide up here in case someone needs to do the same, I had to do a fair amount of searching to get this to work the way I wanted it to. I have a tendency to over complicate things when tinkering.

There is a method whereas the AUR maintainers have already patched VMware 26.x for new kernels.

**Installation:**
```
paru -S vmware-workstation
paru -S vmware-host-modules
```
But... where's the fun in that! I already installed the .bundle from broadcom! This is an Ewok battle-tested guide for running VMware Workstation 26H1 (build 25388281) on Arch Linux / Garuda Linux with linux‑zen kernel 7.1.5-zen1-2-zen

# Overview:

VMware Workstation 26H1 introduces updated kernel modules (vmmon and vmnet) version 418.x, which do not automatically build on Arch/Garuda’s latest kernels.

**This guide covers:**

  Installing VMware Workstation 26H1
  
  Installing matching kernel headers
  
  Using DKMS to build correct 418.x modules
  
  Verifying module versions and load state

**Requirements:**

  Arch Linux or Garuda Linux
  
  linux‑zen kernel (example: 7.1.5-zen1-2-zen)
  
  Matching linux-zen-headers package
  
  VMware Workstation 26H1 .bundle installer
  
  AUR helper (paru recommended)

# Why VMware Fails on Arch/Garuda

VMware Workstation 26H1 expects Module Versions vmmon418.x and vmnet418.x. Arch/Garuda DKMS packages often build 417.x modules (for VMware 17.x) causing version mismatch with vmmon module: expecting 418.0, and getting 417.0.

Additionally, VMware cannot build modules if kernel headers are missing or mismatched.

# My Installation Steps

Download VMware-Workstation-Full-26H1-25388281.x86_64.bundle from [broadcom.com](https://www.broadcom.com/) you will need t setup an account.

1. Install VMware Workstation 26H1 'if not already installed'
   Open Terminal/konsole and navigate to the foler containing the .bundle file to run the commands.

Give yourself permission to execute the file.
```
sudo chmod a+x VMware-Workstation-Full-26H1-25388281.x86_64.bundle
```

Install VMWare
```
sudo ./VMware-Workstation-Full-26H1-25388281.x86_64.bundle
```

2. Install matching kernel headers, Headers must match your kernel version exactly.
```
sudo pacman -S linux-zen-headers
```
4. Install DKMS host modules

Use the mkubecek DKMS https://github.com/mkubecek/vmware-host-modules/blob/master/INSTALL
```
paru -S vmware-host-modules
```

  **Select 2) vmware-host-modules-dkms-git**
  
This builds 418.x modules using VMware’s own source tarballs.

5. Rebuild VMware modules
```
sudo vmware-modconfig --console --install-all
```
6. Verification

Check module versions
```
modinfo vmmon | grep version
modinfo vmnet | grep version
```
version: 418.x.x

Check modules are loaded
```
lsmod | grep vmmon
lsmod | grep vmnet
```
Restart VMware services
```
sudo systemctl restart vmware-networks.service
sudo systemctl restart vmware-usbarbitrator.service
```
Start VMWare, have fun building VMs on Garuda/Arch linux.


Additional Resources

    VMware Workstation Documentation
    Arch Wiki: VMware
    Garuda Linux Kernel Info

I hope this guide helped you if you found it.



