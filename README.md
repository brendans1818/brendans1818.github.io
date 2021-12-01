# About Me
My name is Brendan Moore, and I am a senior at the University of Tulsa
# Arch Install
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
* POST INSTALLATION
  * Log into root
  * User: root 
  * Password: …..
  * pacman -S gnome 
  * To login into gnome —> gnome-shell —wayland
  * In gnome terminal 
  * pacman -S zsh
  * pacman -S openssh
  * ssh ...
  * Systemctl enable gdm.service
  * Alias called gateway to ssh into class gateway
  * Alias hi_arch → echos hello world
* For the video
  * Ifconfig —> shows IP address
  * sudo -lU Michael
  * sudo -lU sal
  * sudo -lU codi
  * sudo -lU brendans1818
  * pacman -S cmatrix
  * ls —color=auto
# DOCKER INSTALL
**PreDocker Install: (commands to install docker on Ubuntu via the powerpoint)**
  - sudo apt update
  - sudo apt install ca-certificates curl gnupg lsb-release
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg \| sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  - echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  - sudo apt update
  - sudo apt install docker-ce docker-ce-cli containerd.io
  - sudo usermod -aG docker bsm2317 
    - This command adds my user to the docker group so I don't need to sudo every time I run a docker command
# OPENVAS/GREENBONE INSTALL
  - https://utulsa.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=75912983-0806-47a5-a3ad-acc9018aaec3
    - The link on panopto is what I followed in order to install openvas
  - sudo apt-get update 
    - make sure everything is up to date
  - sudo apt-get upgrade 
    - install available upgrades of all packages on my machine
  - sudo service docker status 
    - checks to make sure docker is running - which it is
  - sudo docker run -d -p 443:443 --name openvas mikesplain/openvas 
    - installs the container
  - After this open a web browser (chose firefox), go to https://localhost/
  - Will ask about the security risk, go to advance accept risk and continue
  - Log into greenbone using user and password
  - Create a new target (configurations → target)
  - Set up the name, and the IP then create it
  - After creating a target create a scan (Scan → tasks)
  - Change the name, leave everything else default, then click create
  - After creating a scan, click start. Refresh the page a few times to make sure it is running properly. It will show any vulnerabilities it finds. 
  - Side note: if you want to delete and reinstall the docker: 
    - sudo docker stop openvas
      - Stops the docker
  - sudo docker rm openvas 
    - Removes the docker. This allows you to reinstall it if you wish.
# DOCKER COMPOSE
version: '3.3'  
services:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;openvas:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ports:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- '443:443'  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;container_name: openvas  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;image: mikesplain/openvas     
# SCREENSHOTS
[![Screenshot-2021-11-14-135401.png](https://i.postimg.cc/x1b7CFc4/Screenshot-2021-11-14-135401.png)](https://postimg.cc/mPBVpj3Y)
[![Screenshot-2021-11-14-135149.png](https://i.postimg.cc/SQ9bjs3r/Screenshot-2021-11-14-135149.png)](https://postimg.cc/K4mHHZZk)

# WireGuard Install
# Create a DigitalOcean Account
 - Used this link: https://m.do.co/c/4d7f4ff9cfe4
# Create a new Droplet (Ubuntu 20.04 Droplet)
 - Click on explore control panel
 - Click on droplets which is on the sidebar under manage
 - Click create droplet
 - Choose the Ubuntu option under choose an image
 - Choose the shared cpu basic option under the choose a plan
 - Choose the regular intel with SSD, then choose the $5/mo under the cpu options
 - I chose the New York data center 
 - For authentication, I chose the PASSWORD option
# Install Docker 
- (using https://thematrix.dev/install-docker-and-docker-compose-on-ubuntu-20-04/) [using root, so I dont need to use sudo]
- Install the necessary tools
  - apt install apt-transport-https ca-certificates curl software-properties-common -y
- Add a Docker key
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
- Add Docker repo (for 32bit / 64bit OS)
  - sudo add-apt-repository \   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \  $(lsb_release -cs) \ stable"
- Switch to the correct repo
  - apt-cache policy docker-ce
- Install Docker
  - apt install docker-ce -y
- Install Docker-Compose
  - sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- Set the permissions
  - sudo chmod +x /usr/local/bin/docker-compose
# Install Wireguard 
- (using https://thematrix.dev/setup-wireguard-vpn-server-with-docker/)
- Make two folders for wireguard and create a compose file for wireguard
  - mkdir -p ~/wireguard/
  - mkdir -p ~/wireguard/config/
  - nano ~/wireguard/docker-compose.yml
  - thie compose file can be found below  
  - [![first-image-Server.png](https://i.postimg.cc/Fzc5BJYJ/first-image-Server.png)](https://postimg.cc/JH12y0c1)  
  - I changed the time to Chicago/America, I updated the server IP and I updated the peer names.
# Running/Starting WireGuard
- cd ~/wireguard/
- docker-compose up -d
# Screenshots of WireGuard working on my PHONE
- Screenshot of normal IP on my phone:    
- [![IMG-3630.png](https://i.postimg.cc/138chc9T/IMG-3630.png)](https://postimg.cc/PCk81Dvz)  
- Screenshot of VPN IP on my phone:    
- [![IMG-3631.png](https://i.postimg.cc/Z5PYqXKy/IMG-3631.png)](https://postimg.cc/cvHG993s)  
- Screenshot of WireGuard running on my phone  
- [![IMG-3632.jpg](https://i.postimg.cc/TPL6DST9/IMG-3632.jpg)](https://postimg.cc/kVmzrT66)  
# Screenshots of WireGuard working on my PC
- Screenshot of my normal IP on my PC:    
- [![Screenshot-2021-11-29-135632.png](https://i.postimg.cc/151bcMYS/Screenshot-2021-11-29-135632.png)](https://postimg.cc/qgQmpcTD)  
- Screenshot of WireGuard running on my PC:   
- [![Screenshot-2021-11-29-135643.png](https://i.postimg.cc/Dyvdw3ct/Screenshot-2021-11-29-135643.png)](https://postimg.cc/rzHtfb3J)  
- Screenshot of both normal IP and VPN IP on my PC:  
- [![Screenshot-2021-11-29-135603.png](https://i.postimg.cc/QMYRQ3vY/Screenshot-2021-11-29-135603.png)](https://postimg.cc/DWsjhtv1)  
