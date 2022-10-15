Tags: #linux
Created: 2022-10-15 14:10
References: https://youtu.be/DPLnBPM4DhI

# Arch Install
This note describes the proces of installing Arch [[Linux]].

## Getting the ISO
The first step is to get the [[ISO]] of Arch from their website, which is pretty straight forwad, not much to say about this step.

## Initial boot
Create a bootable drive with something like Balena Etcher, insert that drive into the system you want to install to and boot it up from that drive.

## Set up networking
Run `ip addr show` to check all the if you have access to the internet. For wired connections you should see something like this `enp0s3`, which is the ethernet interface and below it there should be an `inet` entry and an [[IP address]], which means you are connected to the internet.

For wireless interfaces there should be no IP address, which eans that you need some extra setup. Here, the `iwctl` command should be used (did not do this part yet, for more info check arch wiki).

## Set up the disk
### Partition
This is the [[UEFI]] method, which is recommended for systems that have UEFI support.

Run `fdisk -l` to find out what the name of your disk is. In this note, the name of the disk is `/dev/sda`. After finding out the name, you need to partition it. You can do this by running `fdisk /dev/sda`, which will launch another prompt.

In this prompt, you can run `p` to see all the partitions, `g` to wipe old disk and create anew partition table. Once you wiped the disk, you need to create a couple of partitions with `n` and set their types with `t`:
1. `Partition number: default` - `First sector: default` - `Last sector: +500M` - `Type: 1 (EFI System)`
2. `Partition number: default` - `First sector: default` - `Last sector: default` - `Type: 43` ([[Linux LVM]])

In order to write the chnages, use `w`, which will finish the partition process.

### Format
To format the EFI partition, run the `mkfs.fat -F32 /dev/sda1`. Next, you need to create a physycal volume for the Linux LVM partition by running `pvcreate --dataalignment 1m /dev/sda2`.

Next up you need to create a [[Volume Group]] with `vgcreate volgroup0 /dev/sda2`. Next up you need to create the [[Logical Volumes]] (which are basically partitions that you use) using these commands: 

```sh
lvcreate -L 10GB volgroup0 -n lv_root
lvcreate -l 100%FREE volgroup0 -n lv_home
modproble dm_mod
vgscan
vgchange -ay
```

This creates two logical volumes, one for root and one for home, which contain 10GB and respectively the rest of the hard disk space.

Now you need to format the volumes to be ext4 and mount them. You can do this with the following commands:

```sh
mkfs.ext4 /dev/volgroup0/lv_root
mount /dev/volgroup0/lv_root /mnt

mkfs.ext4 /dev/volgroup0/lv_home
mkdir /mnt/home
mount /dev/volgroup0/lv_home /mnt/home

mkdir /mnt/etc
genfstab -U -p /mnt >> /mnt/etc/fstab
```

## Instal Arch
```sh
pacstrap -i /mnt base
arch-chroot /mnt
pacman -S linux-lts linux-lts-headers
pacman -S vim base-devel openssh
systemctl enable sshd
pacman -S networkmanager wpa_supplicant wireless_tools netctl
pacman -S dialog
systemctl enable NetworkManager
pacman -S lvm2
```

After all those commands, you need to change the `/etc/mkinitcpio.conf` file by searching for the `HOOKS=(...)` file and adding `lvm2` between `block` and `filesystems`. After changing the file you need to run `mkinitcpio -p linux-lts`.

Edit another file: `/etc/locale.gen` and uncoment your locale (*en_US.UTF-8 UTF-8*). Now run `locale-gen`.

Set the root password with `passwd` and create a new user for yourself with `useradd -m -g users -G wheel baxr` and change its password with `passwd baxr`.

Install `sudo` with `pacman -S sudo` and associate the `wheel` group wit sudo by running `EDITOR=vim visudo` and uncommenting the `%wheel ALL=(ALL:ALL) ALL` line.

## Install GRUB
For UEFI installation you need these packages: `pacman -S grub efibootmgr dosfstools os-prober mtools`

Also for UEFI installations, you need to do this:
```sh
mkdir /boot/EFI
mount /dev/sda1 /boot/EFI
```

TODO: non-UEFI install