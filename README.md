#UEFI ou Legacy(Bios)
```bash
[ -d /sys/firmware/efi ] && echo "Mode de démarrage détecté : UEFI." || echo "Mode de démarrage détecté : BIOS hérité (Legacy)."
```
_____________________________________________________________________________

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
cls && find /etc/apt -type f -regex ".*\(list\|sources\).*" -exec bash -c 'echo -e "\n\033[1;33m=== $1 ===\033[0m\n" ; [ "${1##*.}" = "list" -o "${1##*.}" = "sources" ] && cat -n "$1"' _ '{}' \;
```
# Sourcelist key
```bash
cls && find /etc/apt -type f -regex ".*\(list\|sources\).*" -exec bash -c 'echo -e "\n\033[1;33m=== $1 ===\033[0m\n" ; [ "${1##*.}" = "list" -o "${1##*.}" = "sources" ] && cat -n "$1"' _ '{}' \; && echo -e "\n\033[1;33m=== GPG KEYS /etc/apt/trusted.gpg.d ===\033[0m\n" && ls -la /etc/apt/trusted.gpg.d/ && echo -e "\n\033[1;33m=== GPG KEYS /usr/share/keyrings ===\033[0m\n" && ls -la /usr/share/keyrings/ && echo -e "\n\033[1;33m=== FINGERPRINTS trusted.gpg.d ===\033[0m\n" && for f in /etc/apt/trusted.gpg.d/*.gpg; do echo "--- $f ---"; gpg --no-default-keyring --keyring "$f" --list-keys 2>/dev/null; done && echo -e "\n\033[1;33m=== FINGERPRINTS tiers /usr/share/keyrings ===\033[0m\n" && for f in /usr/share/keyrings/*.gpg; do [[ "$f" != *ubuntu* ]] && echo "--- $f ---" && gpg --no-default-keyring --keyring "$f" --list-keys 2>/dev/null; done && echo -e "\n\033[1;33m=== APT-KEY LIST ===\033[0m\n" && apt-key list 2>/dev/null || echo "apt-key vide ou déprécié"
```

# Sourcelist-full
```bash
cls && find /etc/apt -type f -regex ".*\(list\|sources\).*" -exec bash -c 'echo -e "\n\033[1;33m=== $1 ===\033[0m\n" ; [ "${1##*.}" = "list" -o "${1##*.}" = "sources" ] && cat -n "$1"' _ '{}' \; && echo -e "\n\033[1;33m=== GPG KEYS /etc/apt/trusted.gpg.d ===\033[0m\n" && ls -la /etc/apt/trusted.gpg.d/ && echo -e "\n\033[1;33m=== GPG KEYS /usr/share/keyrings ===\033[0m\n" && ls -la /usr/share/keyrings/ && echo -e "\n\033[1;33m=== FINGERPRINTS trusted.gpg.d ===\033[0m\n" && for f in /etc/apt/trusted.gpg.d/*.gpg; do echo "--- $f ---"; gpg --no-default-keyring --keyring "$f" --list-keys 2>/dev/null; done && echo -e "\n\033[1;33m=== FINGERPRINTS keyrings ===\033[0m\n" && for f in /usr/share/keyrings/*.gpg; do echo "--- $f ---"; gpg --no-default-keyring --keyring "$f" --list-keys 2>/dev/null; done
```
# Sourcelist-full plus orig
```bash
cls && find /etc/apt -type f \( -name "*.list" -o -name "*.sources" -o -name "*.orig" -o -name "*.bak" \) -exec bash -c 'echo -e "\n\033[1;33m=== $1 ===\033[0m\n" ; cat -n "$1"' _ '{}' \; && echo -e "\n\033[1;33m=== GPG KEYS (if exists) /etc/apt/trusted.gpg.d ===\033[0m\n" && ls -la /etc/apt/trusted.gpg.d/ 2>/dev/null && echo -e "\n\033[1;33m=== GPG KEYS (if exists) /usr/share/keyrings ===\033[0m\n" && ls -la /usr/share/keyrings/ 2>/dev/null && echo -e "\n\033[1;33m=== FINGERPRINTS trusted.gpg.d ===\033[0m\n" && for f in /etc/apt/trusted.gpg.d/*.gpg; do [ -e "$f" ] && echo "--- $f ---" && gpg --no-default-keyring --keyring "$f" --list-keys 2>/dev/null; done && echo -e "\n\033[1;33m=== FINGERPRINTS keyrings ===\033[0m\n" && for f in /usr/share/keyrings/*.gpg; do [ -e "$f" ] && echo "--- $f ---" && gpg --no-default-keyring --keyring "$f" --list-keys 2>/dev/null; done
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
# GRUB-LIST

```bash
sudo awk '/submenu /{s=1;si=0;depth=1;match($0,/["\x27][^"'\'']+["\x27]/);printf "%d : %s\n",ti++,substr($0,RSTART+1,RLENGTH-2);next} s{for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{")depth++;if(c=="}")depth--;if(depth==0){s=0;break}}} /menuentry /{match($0,/["\x27][^"'\'']+["\x27]/);name=substr($0,RSTART+1,RLENGTH-2);if(name!=""){if(s){printf "\t> %d : %s\n",si++,name}else{printf "%d : %s\n",ti++,name}}}' /boot/grub/grub.cfg
```
### Ou creer app
```bash

sudo tee /usr/local/bin/grub-list << 'EOF'
#!/bin/bash
sudo awk '/submenu /{s=1;si=0;depth=1;match($0,/["'"'"'][^"'"'"']+["'"'"']/);printf "%d : %s\n",ti++,substr($0,RSTART+1,RLENGTH-2);next} s{for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{")depth++;if(c=="}")depth--;if(depth==0){s=0;break}}} /menuentry /{match($0,/["'"'"'][^"'"'"']+["'"'"']/);name=substr($0,RSTART+1,RLENGTH-2);if(name!=""){if(s){printf "\t> %d : %s\n",si++,name}else{printf "%d : %s\n",ti++,name}}}' /boot/grub/grub.cfg
EOF
sudo chmod +x /usr/local/bin/grub-list
```
### Lancer
```bash
grub-list
```
### Full info grub
```bash
sudo awk '/submenu /{s=1;si=0;depth=1;match($0,/["\x27][^"'\'']+["\x27]/);printf "%d : %s\n",ti++,substr($0,RSTART+1,RLENGTH-2);next} s{for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{")depth++;if(c=="}")depth--;if(depth==0){s=0;break}}} /menuentry /{match($0,/["\x27][^"'\'']+["\x27]/);name=substr($0,RSTART+1,RLENGTH-2);if(name!=""){if(s){printf "\t> %d : %s\n",si++,name}else{printf "%d : %s\n",ti++,name}}}' /boot/grub/grub.cfg && cat /etc/default/grub | grep -v "#" && echo '' && cat /etc/grub.d/40_custom | grep -v "#"
```
### OU
```bash
cls && echo '=== LSBLK ===' && lsblk -fe7 && echo '' && echo '=== MENU GRUB ===' && sudo awk '/submenu /{s=1;si=0;depth=1;match($0,/["\x27][^"'\'']+["\x27]/);printf "%d : %s\n",ti++,substr($0,RSTART+1,RLENGTH-2);next} s{for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{")depth++;if(c=="}")depth--;if(depth==0){s=0;break}}} /menuentry /{match($0,/["\x27][^"'\'']+["\x27]/);name=substr($0,RSTART+1,RLENGTH-2);if(name!=""){if(s){printf "\t> %d : %s\n",si++,name}else{printf "%d : %s\n",ti++,name}}}' /boot/grub/grub.cfg && echo '' && echo '=== CORRESPONDANCE ===' && sudo awk -v blkid="$(sudo blkid | grep -v loop)" 'BEGIN{depth=0;ti=0;si=0;s=0;sdepth=0;inm=0;sp="";oi=0} /submenu /{match($0,/["\x27][^"'\'']+["\x27]/);name=substr($0,RSTART+1,RLENGTH-2);sidx=ti;ti++;s=1;si=0;sp="\t";names[sidx]=name;nums[sidx]=sidx;prefix[sidx]="";uuids[sidx]="";isub[sidx]=1;ord[oi++]=sidx;for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{"){depth++;if(sdepth==0)sdepth=depth}if(c=="}")depth--};next} /menuentry /{match($0,/["\x27][^"'\'']+["\x27]/);name=substr($0,RSTART+1,RLENGTH-2);if(s){curidx=sidx">"si;num=si;si++}else{curidx=ti;num=ti;ti++};names[curidx]=name;nums[curidx]=num;prefix[curidx]=sp;uuids[curidx]="";ord[oi++]=curidx;inm=1;for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{")depth++;if(c=="}")depth--};next} inm && /search --no-floppy --fs-uuid --set=root [^-]/{if(curidx!=""){match($0,/[a-f0-9-]{36}/);uuids[curidx]=substr($0,RSTART,RLENGTH)}} {for(i=1;i<=length($0);i++){c=substr($0,i,1);if(c=="{")depth++;if(c=="}")depth--;if(s&&depth<sdepth){s=0;sdepth=0;sp="";break}};if(/\}/)inm=0} END{for(k=0;k<oi;k++){i=ord[k];if(i in names){dev="none";if(uuids[i]!=""){n=split(blkid,lines,"\n");for(j=1;j<=n;j++){if(index(lines[j],uuids[i])>0){match(lines[j],/^[^:]+/);dev=substr(lines[j],RSTART,RLENGTH);break}}};if(isub[i]){printf "%d : %s (submenu)\n",nums[i],names[i]}else{printf "%s%d : %s\n%s    UUID: %s\n%s    DEV:  %s\n",prefix[i],nums[i],names[i],prefix[i],(uuids[i]!=""?uuids[i]:"none"),prefix[i],dev}}}}' /boot/grub/grub.cfg && echo '' && echo '=== PARTITIONS ===' && sudo blkid | grep -v loop | awk -F': ' '{printf "\n%d : %s\n",NR-1,$1; n=split($2,a," "); for(i=1;i<=n;i++){printf "    %s\n",a[i]}}' && echo '' && echo '=== UUID GRUB ===' && sudo awk -v blkid="$(sudo blkid | grep -v loop)" 'BEGIN{ui=0} /search --no-floppy --fs-uuid --set=root [^-]/{match($0,/[a-f0-9-]{36}/);uuid=substr($0,RSTART,RLENGTH);if(!(uuid in seen)){seen[uuid]=1;dev="none";n=split(blkid,lines,"\n");for(j=1;j<=n;j++){if(index(lines[j],uuid)>0){match(lines[j],/^[^:]+/);dev=substr(lines[j],RSTART,RLENGTH);break}};printf "%d : %s  →  %s\n",ui++,uuid,dev}}' /boot/grub/grub.cfg && echo '' && echo '=== /etc/default/grub ===' && grep -v "^#" /etc/default/grub && echo '' && echo '=== /etc/grub.d/40_custom ===' && grep -v "^#" /etc/grub.d/40_custom
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
### Version Abrégé
```bash
echo; dpkg-query -W -f='${db:Status-Abbrev} ${Package} ${Version} ${Architecture}\n' | grep -v '^rc' | grep -E ' linux-(c|g|h|i|lo|m|si|t)' | sort -k3V | column -t; echo -e "\nNoyau courant : $(uname -mr)"
```
_____________________________________________________________________________
# UUID
```bash
echo "";echo "====/dev/disk/by-uuid/===="; sudo ls -la /dev/disk/by-uuid/* ;echo ""; echo "====blkid -s UUID====";sudo blkid -s UUID ;echo ""; echo "====FSTAB====";cat /etc/fstab | grep -v "^#" ;echo""; echo "====DM-UUID===="; ls -la /dev/disk/by-id/ | grep dm-uuid ;echo ""; echo "====DM-UUID=UUID===="; for dev in /dev/disk/by-id/dm-uuid-* ; do [ -e "$dev" ] || continue; name=$(basename $dev); target=$(readlink -f $dev 2>/dev/null); uuid=$(lsblk -no UUID $target 2>/dev/null); echo "$name = $uuid"; done ;echo "";echo "====FINDMNT====";sudo findmnt / -o SOURCE;echo ""; echo "====df -hT===="; /bin/df -hT | grep -E "ext4|vfat|xfs|btrfs";echo "";echo "====LSBLK===="; lsblk -fe7
```

# MEMOIRE

```bash
watch -n 0.5 "awk '/MemTotal|MemFree|MemAvailable|Buffers|Cached/ {val=\$2/1024; if(val>=1024) printf \"%-20s %6.1f Go\n\", \$1, val/1024; else printf \"%-20s %6.1f Mo\n\", \$1, val}' /proc/meminfo"
```
