# Setup

## Partitionement

``` bash
parted /dev/sda -- mklabel gpt
parted /dev/sda -- mkpart primary 512MiB -6GiB
parted /dev/sda -- mkpart primary -6GiB 100%
parted /dev/sda -- mkpart ESP fat32 1MiB 512MiB
parted /dev/sda -- set 3 boot on
lsblk
cryptsetup luksFormat /dev/sda1
cryptsetup luksOpen /dev/sda1
cryptsetup luksOpen /dev/sda1 nixos-root
ls -l /dev/mapper
mkfs.btrfs -L nixroot /dev/mapper/nixos-root
ls -l /dev/mapper
mkswap /dev/sda2
swapon /dev/sda2
free -hm
mkfs.vfat -n BOOT /dev/sda3
btrfs subvolume create /mnt/nixos
btrfs subvolume create /mnt/home
btrfs subvolume create /mnt/var
btrfs subvolume create /mnt/nix
mkdir /mnt/boot
mount /dev/sda3 /mnt/boot
mount -t btrs -o subvol=nixos /dev/mapper/nixos-root /mnt
```

## System conf

System conf are in /etc/nixos/.
There is an hardware conf and a system conf.

Once the configuration is updated we need to apply it with : `nyxos-rebuild`
