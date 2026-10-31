# Installation Document of Arch Linux  
## Step 1: *Download the ISO*  
[Arch Linux ISO](https://codingflyboy.mm.fcix.net/archlinux/iso/2026.10.01/)

## Step 2: *Verify the Integrity of ISO by Hashing*
#### Hash the ISO file in anyway and compare hashed file with the hash below. If they match you may proceed to the next step. 
--> Note: **IF THEY DON'T MATCH, REDOWNLOAD FILE.**

     SHA256:684ded26c63240ff4a41e8c25ee84ea6da233f557364821f13d12c2b0a9059a5

## Step 3: *Open VMware Workstation Pro*
### A.  *Create New Virtual Machine*
### B.  *Click next and click option*
    o Installer dic image file(iso):
### C.  *Browse for Arch Linux Iso and then click next.*
### D. *In the next tab, choose the option linux and version:*
    Other Linus 6.x Kernel 64-bit
### E. *Click next Twice*
### F. *Set Disk Size to 30GB. Next.*
### G. *Customize the instance to use 4GB of RAM. Then Finish*

## Step 4: *Power on Virtual Machine BOOT into BIOS by Pressing ENTER*
## Step 5: Checking Internet Connection 
 **To check internet connection type the two commands:** 

    ip addr show 

    ping -c 5 8.8.8.8
**If your system is able to retrive the packets and you have an ip address you are connected to the Internet.*

## Step 6: *Partitioning the disk*
### A. Use the command to verify disk is assigned a block device.
    fdisk -l
### B. Now to start partitioning use the command:
    fdisk /dev/sda
### C. Follow these steps once in fdisk:
    1. Enter "g" to make a GPT Partition Table for UEFI
    2. Make a new partition by entering 'n" 
    3. Use default by pressing ENTER.
    4. Use default 1st size sector and then type for last sector "+1G" Then ENTER.
    5. Repeat Twice, steps (2-4). Although for the 2nd partition use "+4G" and 3rd Partition press "ENTER" to use default.
    6. Finally enter "p" to check if table was set up correctly with the apporiate size.
    7. Change Partition Type by entering "t" and then start with partition "1" and set it to "1"
    8. Repeat step 7, doing partitions 2 and 3. where the 2nd partition is set with number "19" and 3rd is set with number "23"
    9. Enter "p" again to check table type names. Now press "w" to write.
## Step 7: *Format the Partitions*
Once the partitions have been created, each newly created partition must be formatted with an appropriate file system.

**To create an Ext4 file system on /dev/root_partition, run:**

    mkfs.ext4 /dev/sda3

**Partition for swap, initialize it with mkswap:**

    mkswap /dev/sda2
**If you created an EFI system partition, format it to FAT32 using mkfs.fat(8).**

    mkfs.fat -F 32 /dev/sda1
## Step 8: *Mount the File System*
**Mount the root volume to /mnt.**

    mount /dev/sda3 /mnt

**Mount the EFI system partition. For example:**

    mount --mkdir /dev/sda1 /mnt/boot
**Swap volume, enable it with swapon:**

    swapon /dev/sda2
## Step 8: *Install essential packages*
**Use the command to download necessary packages like the kernel, networkmanager, console text editor and documentation packages:**

    pacstrap -K /mnt base linux linux-firmware networkmanager nano vim man-db man-pages texinfo sudo
## Step 9: *Configure the System*
### A. *Fstab*
**For needed file systems. mounted on startup, generate an fstab file with persistent block device naming using genfstab:**

    genfstab -U /mnt >> /mnt/etc/fstab


### B. *Chroot*
**To interact with the new system environment, tools, and configurations you change root into the new system:**

    arch-chroot -S /mnt
*Note\* You are now Interacting with the Enviroment after executing this command.*
### C. *Time*
**For setting the time zone:**

    ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
**Run hwclock(8) to generate /etc/adjtime:**

    hwclock --systohc --localtime
### D. *Localization*
**Setting correct region and language formatting (Dates, Currency, Decimal Spearators). First nano into */etc/local.gen* and uncomment "en_US.UTF-8"**

    nano /etc/locale.gen 

**Now generate the local:**

  locale-gen

**Create locale.conf file and set the LANG variable:**

    echo "LANG=en_US.UTF-8" > /etc/locale.conf
## Step 10: *Network configuration*
**Assigns a consistent Name to system (Good for network management")**

    echo "ArkkyChan" > /etc/hostname
**Also enable NetworkManager**

    systemctl enable NetworkManager

## Step 11: *Set ROOT Password*
**Set a password for the root user to allow performing administrative actions:**

    passwd [insert password]

## Step 12: *Set Users*
The 2 commands going to be executed makes users and a group for users with a subgroup of "wheel" for sudo permissions. ( *Set users [ Arkchan, codi ]* )

    useradd -m -g users -G wheel ArkChan
    useradd -m -g users -G wheel codi

Go and use the "passwd" command in the same line add one of the user and set the password twice. Do this for each user. We also want to set anyone in the wheel group with sudo priviledges. Thus:

nano visudo

Just scroll and uncomment the "#" from the line containing:

    # %wheel ALL=(ALL:ALL) ALL

Save with Ctrl + S and then exit with Ctrl + X.

## Step 13: *Additional Packages*
**Use the command to download necessary packages like the kernel, networkmanager, console text editor and documentation packages:**

    pacman -S base-devel dosfstools grub efibootmgr gnome gnome-tweaks mtools  networkmanager os-prober openshh
     

## Step 14: *Set Up BOOT Loader*

Here's is where I set my personal install command for EFI (This installs grub itself):

 
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --recheck

Then the below command sets up the configuration for grub:

    grub-mkconfig -o /boot/grub/grub.cfg

## Step 15: *Enable Gnome and REBOOT*
To enable our desktop enviroment we installed in step 13 use:

    systemctl enable gdm

Now the entire setup is done just type "reboot" to reboot.

## Step 16: *Login into User and Open Terminal*

Now just login into ArkkyChan with passwd:

    1234

And finally open console to start installing Zsh.

## Step 17: *Install Zsh & Adding Theme + Color Coding*

The defualt shell is BASH and thats for losers so we are changing to Zsh becuase we are cool. 

### A. *Command for Install zsh plus other required packages for later.*

    sudo pacman -S zsh zsh-completions git curl

### B. *Change Shell Default*

    chsh -s $(which zsh)

After this just run the command "zsh" to make sure  you in the zsh shell now.

### C. *Oh my zsh plugins and themes install*  

    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

### D. *Additional Plugins for quality of life. (This adds color coding too)*

    sudo pacman -S zsh-syntax-highlighting zsh-autosuggestions

### E. *Enabling Plugins*

#### 1. Nano into zshrc file

    nano ~/.zshrc

#### 2. Serch for plugins line "plugins=()" and add in brackets:

    plugins=(git sudo zsh-syntax-highlighting zsh-autosuggestions)

### F. *Change theme to bira*

Within the same text editor and file look for the name with the line ZSH_THEME. And change the quotes of the line to look like this.

    ZSH_THEME="bira"

Save and then Exit.

### Step 18: *Adding Alias*

Adding some alias for quality of life.

    alias ls='ls --color=auto'

    alias grep='grep --color=auto'

    alias egrep='egrep --color=auto'

    alias fgrep='fgrep --color=auto'

### Step 19: *DONE YAYYY, GET A CHOCOLY MILK :D*