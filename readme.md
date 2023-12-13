# NixOS install

## Partition the drives

## Encrypt the main drive

```
$ cryptsetup luksFormat /dev/nvme0p1 --type luks2
$ cryptsetup luksOpen /dev/nvme0p1 crypted
```

## Creating logical volumes

```
$ pvcreate /dev/mapper/crypted
$ vgcreate vg /dev/mapper/crypted
$ lvcreate -L 32G -n swap vg
$ lvcreate -l '100%FREE' -n nixos vg
```

## Format disks

```
$ sudo mkfs.fat -F 32 -n boot /dev/sda1
$ sudo mkfs.ext4 -L nixos /dev/vg/nixos
$ sudo mkswap -L swap /dev/vg/swap
```

## Mount and generate config

```
$ mount /dev/disk/by-label/nixos /mnt
$ mkdir -p /mnt/boot
$ mount /dev/sdb1 /mnt/boot
$ swapon /dev/vg/swap
```

## Generate config

```
nixos-generate-config --root /mnt
```

To retrive the UUID
```
blkid /dev/nvme0p1 -s PARTUUID
```

## Install

```
nixos-install --root /mnt --cores 0
```

# Sources
[1] https://shen.hong.io/installing-nixos-with-encrypted-root-partition-and-seperate-boot-partition/
