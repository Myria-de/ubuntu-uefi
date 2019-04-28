# Ubuntu/Bios nach Ubuntu/Uefi umziehen

Die folgenden Befehlszeilen beschreiben, wie man ein im Bios/Legacy-Modus installiertes Ubuntu in eine Uefi-Installation auf einer anderen Festplatte umwandelt.

## Deutsches Tastaturlayout für Ubuntu bei der Uefi-Installion
console-setup/layoutcode?=de

## Linux mit Uefi neben Windows einrichten

Adminrechte im Terminal verschaffen:
```
sudo -i
```

Infos zu Partitionen ermitteln:
```
blkid
```

Partitionen einhängen
```
mkdir /mnt/neu /mnt/alt
mount /dev/sda5 /mnt/neu
mount /dev/sdb1 /mnt/alt
```

Linux-Installation kopieren:

```
rsync --stats --progress --numeric-ids -axAhHSP /mnt/alt/ /mnt/neu/
```

Von Windows bereits vorhandene EFI-Partition einbinden:
```
mkdir /mnt/neu/boot/efi
mount /dev/sda2 /mnt/neu/boot/efi
```

In die chroot-Umgebung wechseln
```
for dir in /dev /dev/pts /proc /sys /run; do sudo mount --bind $dir /mnt/neu/$dir; done 
chroot /mnt/neu /bin/bash
```

Grub löschen:
```
apt-get purge grub-common grub-pc os-prober
```

Grub für Uefi neu installieren, Grub installieren, updaten und chroot verlassen:

```
apt-get install  grub-common grub-efi-amd64 os-prober 
grub-install 
update-grub
exit
```
fstab im Editor öffnen:
```
gedit /mnt/neu/etc/fstab
```
