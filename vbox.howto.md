## **Installing VirtualBox on Arch Linux**

This is a quick guide to install, configure and run VirtualBox on Arch Linux.
For this guide, I will be using Ubuntu 14.04 LTS as a guest system

### **Installation**

#### **Install**
```bash
$ pacman -S virtualbox virtualbox-host-dkms dkms linux-headers
```
#### **Run **
```sh
$ dkms install vboxhost/5.0.14
```
or
```bash
$ dkms install vboxhost/$(pacman -Q virtualbox|awk '{print $2}'|sed 's/\-.\+//') -k $(uname -rm|sed 's/\ /\//')
```
To enable it on boot:
```bash
$ systemctl enable dkms.service
```
---
### **Modules**

#### **Mandatory Module to be loaded before stating virtualization**

Manually
```bash 
$ modprobe vboxdrv
```

On boot - Edit or create the file
>/etc/modules-load.d/virtualbox.conf
>```
>vboxdrv
>```

#### **Optional modules**

* To use bridged or host-only networking
 * `vboxnetadp`
 * `vboxnetftl`

* To pass through a PCI device
 * `vboxpci` 





---
### **Utilities**
For now, the VMs you create should already work, but for extra functionality (e.g set higher resolution), the guest system should have Guest Additions installed

#### **From Arch host**

Install `virtualbox-guest-iso` on host system

```bash
$ pacman -S virtualbox-guest-iso
```
The *.iso* file is located at `/usr/lib/virtualbox/additions/VBoxGuestAdditions.iso`. Mount it inside the guest system by adding a new optical drive under the *storage* category and pointing it to the *.iso* image.
  
Once mounted, boot your VM and go into the drive directory (e.g. `/media/$USER/VBOXADDITIONS_X.X.XX_XXXXXX` on ubuntu)

Run the *.run* or *.exe* executable corresponding to the guest system, with administrative priviledges.

```bash
$ #On Linux
$ sudo ./VBoxLinuxAdditions.run
```
Reboot your VM.
 
#### **From within the guest system**

You can also install Guest Additions from inside the running VM. On Ubuntu it would be:

```bash
$ sudo apt-get install virtualbox-guest-additions-iso
```
Although, the first method is recommended as the Guest Addition's version will match your hosting VirtualBox version.

#### **USB**
To use USB ports, add usernames to vboxusers group
```bash
$ gpasswd -a $USER vboxusers
```

---
### **CLI**

#### VBoxSDL

To use vbox from CLI, use the `VBoxSDL` command. Provides simple window with no menus.


