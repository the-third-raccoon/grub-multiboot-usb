## Linux ISO Files

This section describes the setup for booting Linux ISO files. 

The process follows directly from [uGrub](https://github.com/adi1090x/uGRUB). An abridged set of instructions is provided below along with configurations for **Ubuntu** and **Linux Mint** ISOs. If you have an ISO for a different Linux OS, please refer to `grub.cfg` as provided by [uGrub](https://github.com/adi1090x/uGRUB).

### Mount the USB Partition
1. Open a terminal and mount the `ISO` partition on the USB. 

    In this example, the partition is `/dev/sda3` (as seen in `GParted`). Replace this with the appropriate partition for your system. 

    ```
    sudo mount /dev/sda3 /mnt
    ```

### Copy ISO Files to the USB
1. Open a terminal and navigate to the directory containing the Linux ISO files. Replace `ubuntu-24.04.3-desktop-amd64.iso` with the appropriate filename for your setup. 

    ```
    sudo rsync -avh --progress ubuntu-24.04.3-desktop-amd64.iso /mnt/
    ```

2. Repeat for all Linux ISOs.

### Unmount the USB Partition
1. Unmount the `ISO` partition.

    ```
    sudo umount /mnt
    ```

### Update GRUB2 Configuration
1. Identify the UUID of the `ISO` partition.

    ```
    sudo blkid
    ```

    You should see output similar to:

    ```
    ...
    /dev/sda3: LABEL="ISO" UUID="709858ec-a703-4a1f-8b6f-bcebeeec0381" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="ISO" PARTUUID="7ecaced5-e3a7-4240-974b-6d660dc23be9"
    ...
    ```

    Find the line corresponding to your `ISO` partition and take note of the UUID. In this example, it is `709858ec-a703-4a1f-8b6f-bcebeeec0381`.
2. Replace the content of `grub.cfg` with the following configuration (substitute your UUID as needed).

    ```
    set timeout=10
    set default=0

    menuentry "Reboot" {
        reboot
    }

    menuentry "Shutdown" {
        halt
    }

    menuentry "Windows 11" {
        insmod part_gpt
        insmod fat
        search --no-floppy --fs-uuid --set=root 3D0B-03DF
        chainloader /efi/boot/bootx64-windows.efi
        boot 
    }

    menuentry "Ubuntu 24.04.3" {
        search --no-floppy --fs-uuid --set=root 709858ec-a703-4a1f-8b6f-bcebeeec0381
        set isofile="/ubuntu-24.04.3-desktop-amd64.iso"
        loopback loop $isofile
        linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=${isofile} quiet splash vt.global_cursor_default=0 loglevel=2 rd.systemd.show_status=false rd.udev.log-priority=3 sysrq_always_enabled=1 cow_spacesize=1G
        initrd (loop)/casper/initrd
    }

    menuentry "Linux Mint Live ISO" --class linuxmint --class linux {
        search --no-floppy --fs-uuid --set=root 709858ec-a703-4a1f-8b6f-bcebeeec0381
        set isofile="/linuxmint-22.3-cinnamon-64bit.iso"
        loopback loop $isofile
        linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=${isofile} quiet splash vt.global_cursor_default=0 loglevel=2 rd.systemd.show_status=false rd.udev.log-priority=3 sysrq_always_enabled=1 cow_spacesize=1G
        initrd (loop)/casper/initrd.lz
    }
    ```

### Setup Complete

If all has gone well, you should now be in possession of a Windows + Linux multiboot USB. Yay!

---
<table width="100%">
<td align="left"><a href="windows-installer.md">⟸ Windows Installer</a></td>
<td align="right"><a href="references.md">References ⟹</a></td>
</table>