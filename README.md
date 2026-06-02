# bootinfo
## FULL
```bash
sudo bash -c 'echo "========== SYSTEM ==========" ; uname -a ; hostname ; cat /etc/os-release ; uname -m ; uname -r ; grep -m1 "model name" /proc/cpuinfo ; free -h ; cat /proc/meminfo | head -5 ; echo "========== DISKS ==========" ; lsblk -f ; lsblk -o NAME,FSTYPE,MOUNTPOINT,SIZE,UUID,LABEL,PARTUUID ; cat /proc/partitions ; sudo fdisk -l 2>/dev/null ; sudo parted -l 2>/dev/null ; sudo blkid ; df -hT ; findmnt --fstab --raw ; echo "========== BOOT-INFO ==========" ; cat /proc/cmdline ; sudo efibootmgr -v 2>/dev/null ; findmnt -n -o SOURCE,TARGET,FSTYPE,OPTIONS /boot/efi ; ls -la /boot/efi/EFI 2>/dev/null ; echo "========== GRUB ==========" ; cat /etc/default/grub ; echo "========== ls GRUB.D ==========" ; ls -l /etc/grub.d ; echo "========== GRUB CFG ==========" ; sudo cat /boot/grub/grub.cfg 2>/dev/null || sudo cat /boot/grub2/grub.cfg 2>/dev/null ; echo "========== GRUB THEMES ==========" ; ls -la /boot/grub/themes 2>/dev/null || ls -la /boot/grub2/themes 2>/dev/null ; echo "========== SECUREBOOT ==========" ; sudo mokutil --sb-state 2>/dev/null ; sudo dmesg | grep -iE "secureboot|efi" | tail -20 ; echo "========== TPM ==========" ; ls /sys/class/tpm/ 2>/dev/null ; sudo dmesg | grep -i tpm | tail -20 ; echo "========== LUKS ==========" ; lsblk -f | grep crypto_LUKS ; sudo cat /etc/crypttab 2>/dev/null ; echo "========== RAID ==========" ; cat /proc/mdstat ; echo "========== FILESYSTEM ==========" ; sudo cat /etc/fstab ; sudo swapon --show ; lsblk -o NAME,FSTYPE,MOUNTPOINT,SIZE,UUID ; echo "========== LOGS ==========" ; sudo journalctl -p 3 -xb --no-pager -n 50 ; sudo dmesg | tail -50'| curl -F 'content=<-' https://dpaste.com/api/v2/
```
## Partiel
```bash
sudo bash -c 'echo "===== SYSTEM ====="; uname -a; hostname; cat /etc/os-release; uname -m; uname -r; grep -m1 "model name" /proc/cpuinfo; free -h; head -5 /proc/meminfo; echo "===== DISKS ====="; lsblk -f; lsblk -o NAME,FSTYPE,MOUNTPOINT,SIZE,UUID,LABEL,PARTUUID; cat /proc/partitions; fdisk -l 2>/dev/null; parted -l 2>/dev/null; blkid; df -hT; findmnt --fstab --raw; echo "===== BOOT ====="; cat /proc/cmdline; efibootmgr -v 2>/dev/null; findmnt -n -o SOURCE,TARGET,FSTYPE,OPTIONS /boot/efi; ls -la /boot/efi/EFI 2>/dev/null; echo "===== GRUB ====="; cat /etc/default/grub; ls -l /etc/grub.d; echo "===== SECUREBOOT ====="; mokutil --sb-state 2>/dev/null; dmesg | grep -iE "secureboot|efi" | tail -20; echo "===== TPM ====="; ls /sys/class/tpm/ 2>/dev/null; dmesg | grep -i tpm | tail -20; echo "===== LUKS ====="; lsblk -f | grep crypto_LUKS; cat /etc/crypttab 2>/dev/null; echo "===== RAID ====="; cat /proc/mdstat; echo "===== FILESYSTEM ====="; cat /etc/fstab; swapon --show; lsblk -o NAME,FSTYPE,MOUNTPOINT,SIZE,UUID; echo "===== LOGS ====="; journalctl -p 3 -xb --no-pager -n 50; dmesg | tail -50' 2>&1 | curl -F 'content=<-' https://dpaste.com/api/v2/
```
_____________________________________________________________________________
# Sourcelist

```bash
find /etc/apt -type f -regex ".*\(list\|sources\).*" -exec bash -c 'echo -e "\n\t$1\n" ; [ "${1##*.}" = "list" -o "${1##*.}" = "sources" ] && cat -n "$1"' _ '{}' \;
```
_____________________________________________________________________________
# CHROOT

```bash
sudo mkdir -p /mnt
sudo mkdir -p /mnt/boot/efi
sudo mount /dev/nvme0n1p8 /mnt
sudo mount /dev/nvme0n1p1 /mnt/boot/efi
sudo mount --bind /dev /mnt/dev
sudo mount --bind /dev/pts /mnt/dev/pts
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo mount --bind /run /mnt/run
sudo mount --bind /sys/firmware/efi/efivars /mnt/sys/firmware/efi/efivars
sudo chroot /mnt
os-prober
update-grub
grub-install --efi-directory=/boot/efi --target=x86_64-efi --bootloader-id=ubuntu
exit
sudo umount -R /mnt
sudo reboot
```

_____________________________________________________________________________
# Test GRUB

## Installer tous les paquets nécessaires
```bash
sudo apt update && sudo apt install -y \
qemu qemu-system-x86 qemu-utils ovmf seabios mtools xorriso \
libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```
## Mettre à jour GRUB avec ton thème personnalisé
```bash
sudo update-grub
```
## Créer l'ISO bootable à partir de /boot/grub
```bash
sudo grub-mkrescue -o ~/test.iso /boot/grub --compress=xz
```
## Lancer l'ISO dans QEMU en mode UEFI (thème inclus)
```bash
qemu-system-x86_64 -enable-kvm -m 2048 -cdrom ~/test.iso -boot d -bios /usr/share/OVMF/OVMF_CODE.fd
```

_____________________________________________________________________________
# KERNEL INSTALLER
```bash
echo;dpkg-query -W -f='${Status} ${Package} ${Version} ${Architecture}\n' | grep -v '^rc' | grep -E ' linux-(c|g|h|i|lo|m|si|t)' | sort -k3V | column -t;echo -e "\nNoyau courant : $(uname -mr)"
```
