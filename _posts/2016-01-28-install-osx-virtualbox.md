---
title: Install Steps - MacOSX on VirtualBox 
updated: 2016-01-28 22:43
---

## Steps: 

* Install MacOSX Yosemite 
* Run Optimization 

## Installation 

Download the virtual iamge from link.  

<iframe width="560" height="315" src="https://www.youtube.com/embed/DYMEb0ZCfes" frameborder="0" allowfullscreen></iframe>

Go to Disk Utility > Partition > Erase 

* Create MacintoshHD
* Bug with 2 min remaining, reset virtual machine 

Restart 

```
boot: -s -v -n <enter>

/sbin/fsck -fy (checking volumes, etc) 
/sbin/mount -uw / 
```

```
cd /.OSInstallSandboxPath/Scripts/
ls
cd Hackintosh.Zone.Post-Script.<id>/
./postinstall
exit
```

NOTE: Machine can freeze, just restart and -s -v -n (exit) 

## Optimizations 

Graphics Optimization 

* Mouse Runs smoother 
* Go Download Folder > BeamOff

Better Screen Resolution 

* Run Command 
```
vboxmanage setextradata MacOSX-Dev "CustomVideoMode1" "1920x1080x32"
```
* Change file 

## Know Issues 

* El Captain does not work (hangs on Apple ID Sign-In) 
* Mac OS X guests only works with one CPU.  Support for SMP will be provided in a future release.

### Issue: Modify CPU ID Set 

```
VBoxManage modifyvm "MacOSX-Dev" --cpuidset 00000001 000306a9 00020800 80000201 178bfbff
```

### Issue: Bootup Parameters 

```
-x -v (safe mode) 
```

### Issue: Spinning Wheel at Login 

Run the modifyvm CPU ID set command 