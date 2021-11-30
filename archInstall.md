**Steps to install Arch by Brendan Moore**
* I set the .vmx file in line 2→ firmware=”efi”
* Next I verified the boot mode.
  * Using command ls /sys/firmware/efi/efivars (ran smoothly verifying the boot mode)
* Checked the internet connection
  * Using command ping archlinux.org  (sent packets to archlinux.org)
* I set the time
  * timedatectl set-ntp true
* Next I partitioned the disks, using the recommended setting given in the powerpoint
  * Using the fdisk -l i see all the available drives
  * Next i use cfdisk to partition the drive → cfdisk /dev/sda (allows me to create a partition)
  * Select label type → dos
  * Hit enter while on the [new] button at the bottom
  * Set the partition size for sda1 to 0.5G, hit enter to confirm
  * I then changed the default type and made sda1 EFI type
  * Next hit primary and it took me back to the screen showing the /dev/sda1
  * Next I created a second partition. I did this by clicking down on the arrow key and then choosing new and following the same steps (label type dos) then gave it the rest of the space. Then I clicked primary and i have both partitions set up.
  * I then kept the default type for sda2 → linux
  * After all of this I created a file system. 
   *  mkfs.ext4 /dev/sda2 (makes a file system)
   *  To wipe a fs → wipefs -a /dev/sda2 (I accidentally put the fs on /dev/sda1 so I needed to remove it to put it on /dev/sda2) 
 * After that, i mounted the file system 
  * mount /dev/sda1 /mnt (mounts to a file system)
* After all the partitioning and mounting I looked at mirrors but didnt add them/change anything.
* Next, I installed essential packages. 
  * pacstrap /mnt base linux linux-firmware (installed things needed for linux)
* After that, I configured the system
  * genfstab -U /mnt >> /mnt/etc/fstab (helps fill in an fstab file)
* Change root
  * arch-chroot /mnt (changed to root)
* Change time
  * timedatectl set-timezone US/Central
* For whatever reason sudo, and nano were NOT included
  * To include them: pacman -S sudo (installs sudo)
  * Pacman -S nano (installs nano)
* After this i did localization
  * sudo nano /etc/locale/gen (allows you to edit)
  * Uncommented en_US.UTF-8 UTF-8 
  * Saved this, then did locale-gen and it generated locales
  * After this i created the locale.conf 
    * echo LANG=en_US.UTF-8 > /etc/locale.conf
* After localization, I created a host name for network configuration
  * echo brendans1818 > /etc/hostname
  * Created a host file → touch /etc/hosts 
    * 127.0.0.0 localhost
    * ::1 localhost
    * 127.0.1.1 brendans1818
  * Enabled DHCP
    * Was NOT installed → pacman -S dhcpcd
    * systemctl enable dhcpcd
* Changed root’s password
  * passwd
* Installed a boot loader.
  * Looking at the bootloader page, GRUB seemed to be the best option
  * To install grub (the bootloader): 
    * pacman -S grub
    * pacman -S efibootmgr
    * Format EFI partition
      * pacman -S dosfstools
      * mkfs.fat -F32 /dev/sda1
    * Mount EFI partition
      * mkdir /efi
      * mount /dev/sda1 /efi
    * Install grub
      * grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
    * To generate grub.cfg 
      * grub-mkconfig -o /boot/grub/grub.cfg
* FINALLY
  * Reboot
