Petitboot for coreboot
======================

## About petitboot
Petitboot is a platform independent bootloader based on the Linux kexec warm reboot mechanism.Petitboot supports loading kernel, initrd and device tree files from any Linux mountable filesystem, plus can load files from the network using the FTP, SFTP, TFTP, NFS, HTTP and HTTPS protocols. Petitboot can boot any operating system that includes kexec boot support.

Petitboot includes graphical and command line user interface programs. The command line programs can be used to boot the system remotely via telnet or ssh sessions. Multiple user interface programs can run simultaneously.

Petitboot is licensed under the GPLv2.

See [Petitboot website](https://www.kernel.org/pub/linux/kernel/people/geoff/petitboot/petitboot.html) for more information.

## About petitboot for coreboot
This builds a bzImage with a linked initramfs that contains petitboot.
Currently, the kernel is trimmed down to fit in a the 8MB of a thinkpad x200,
while maintaining essential functionality.
This is done using an industry standard tool for building embedded linux distributions, buildroot.

It is inspired by the op-build package which builds openpower boot firmware, also containing petitboot

## How to build
First get buildroot:

```bash
git submodule update --init
```

Then source petitboot-env

```bash
source petitboot-env
```

Configure and let buildroot do its thing

```bash
cd buildroot
make petitboot_defconfig
make
```

The resulting bzImage will be in `output/images/bzImage`

## How to integrate with coreboot
The simplest way is it add it as `img/petitboot` and load it from SeaBIOS. That way you have a fallback mechanism in case something is wrong with petitboot.

```bash
cbfstool coreboot.rom add-payload -f bzImage -n img/petitboot -C [command line arguments]
```

Another way is to use linux as a payload in coreboot.

## Known problems
You can sometimes only see a part of the petitboot tui. Hitting CTRL + l to clear the screen fixes this.
Adding `console=ttyS1` to the linux command line arguments also fixes this problem, by having the boot messages on a different, unused console.

## Customize buildroot, linux
To customize buildroot, run in the petitboot\_for\_coreboot directory:

```bash
make -C buildroot/ menuconfig
```

To customize linux, run:

```bash
make -C buildroot/ linux-menuconfig
```
