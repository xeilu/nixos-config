# NixOS install

## Partition the drives

## Encrypt the main drive

```
$ cryptsetup luksFormat /dev/sda2
$ cryptsetup luksOpen /dev/sda2 crypt
```

## Creating logical volumes

```
$ sudo pvcreate /dev/mapper/crypted
$ sudo vgcreate vg /dev/mapper/crypted
$ sudo lvcreate -L 32G -n swap vg
$ sudo lvcreate -l '100%FREE' -n nixos vg
```

## Format disks

```
$ sudo mkfs.fat -F 32 -n boot /dev/sda1
$ sudo mkfs.ext4 -L nixos /dev/vg/nixos
$ sudo mkswap -L swap /dev/vg/swap
```

## Mount and generate config

```
$ mount /dev/vg/root /mnt
$ mkdir /mnt/boot
$ mount /dev/sda1 /mnt/boot
$ swapon /dev/vg/swap
```

## Generate config

```
nixos-generate-config --root /mnt
```

## Install

```
nixos-install --root /mnt --cores 0
```
