# Raspberry Pi and NodeOS

## Developing with QEMU and VirtualBox

For development on MacOS X, it can make sense to use Virtualbox and QEMU.

- QEMU to run the `arm` emulation
- Virtualbox to run Ubuntu or another distro that can edit `ext4` partitions

### VirtualBox

Download a base image `RaspiNodeOS.img` and link it to your VirtualBox Ubuntu instance:

```
VBoxManage internalcommands createrawvmdk -filename nodeos.vmdk -rawdisk RaspiNodeOS.img
```

In the VirutalBox settings panel for your instance, attach the `nodeos.vmdk` image as a new disk.
The disk should appear as `/dev/sdb`:

- `/dev/sdb1` should be the `/boot` parition
- `/dev/sdb2` should be your main `ext4` partition housing the NodeOS build

Edit to your hearts content! Don't foreget to unmount before booting QEMU.

### QEMU

Follow the setup isntructions from [Emulating Raspberry Pi the easy way][1].
Note that you *must* download a linux kernel image to boot the instance.

The following *should* boot your system:

```
qemu-system-arm -kernel kernel-qemu -cpu arm1176 -m 256 -M versatilepb -no-reboot -serial stdio -append "root=/dev/sda2 rootfstype=ext4 init=/nodeos/root/bin/init BOOT=1 HOME=/nodeos/root PATH=/nodeos/root/bin:/nodeos/bin npkg start nodeos-knockout" -hda RaspiNodeOS.img
```

[1]: http://xecdesign.com/qemu-emulating-raspberry-pi-the-easy-way/
