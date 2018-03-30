---
layout: post
title: "Dual boot Arch Linux on MacBook Pro Installation"
date: 2013-05-31 14:06
comments: true
categories: 
- arch linux
- macbookpro
- linux
- code
---

The installation process of Arch Linux on a [Macbook Pro](http://www.amazon.com/gp/product/B0074703CM/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0074703CM&linkCode=as2&tag=codelitt-20) has quite a few caveats, but it is about the slickest machine I've ever run. The only difference in my hardware is the [Samsung SSD](http://www.amazon.com/gp/product/B009NB8WRU/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B009NB8WRU&linkCode=as2&tag=codelitt-20), but I point out what to leave off if you have an normal HD. The Arch Linux Wiki entry for Macbooks has gotten pretty messy and has a lot of conflicting information. While I'm going to do my best to clean it up and add information that I found helpful, I wanted to document my process as it may be helpful for others as well as provide a personal resource for future reference. I got a lot of good information from other sources so I try to credit them where applicable, but a great resource especially worth mention is this [post](http://vec.io/posts/use-arch-linux-and-xmonad-on-macbook-pro-with-retina-display).

Having an extra computer handy for Google and an ethernet plugged into your Macbook are definitely useful items to have. 

### Preparing and booting from live USB

The first caveat is with the installation media. It isn't clear anywhere else, but you do need to do a few specific things to create a [live USB which will boot with Apple's UEFI.](https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface#Create_UEFI_bootable_USB_from_ISO)  

These liveUSB instructions are for Linux. May work in OSx, but might as well just fire up a VM to be safe. 

* First create a partition table with at least one partition on the USB.

* Mount the Arch Linux ISO image.

` mkdir -p /mnt/{usb,iso} `

` mount -o loop archlinux-2012.05.03-dual.iso /mnt/iso `

* Create a FAT32 filesystem in the partition on the USB. Change LABEL in the code to the one used in the Arch ISO configuration. You'll find it at:

` /mnt/iso/loader/entries/archiso-x86_64.conf `

To make the FAT32 filesystem:

` mkfs.vfat -F32 /dev/sdXY -n LABEL # e.g. ARCH_201305 `

* Mount the newly created FAT32 USB partition and copy the contents of the instalation media to the USB media.

`mount /dev/sdXY /mnt/usb `

`cp -a /mnt/iso/* /mnt/usb`

`sync`

`umount /mnt/{usb,iso}`

Now you should be able to plug in the USB to your Macbook, hold the option key, and select to boot from the Arch Linux live USB. 

### Partitioning

People usually get stuck on partitioning as well. There are many options, but the simplest is the best in my opinion. Keep your EFI partition from the Mac (don't create a separate EFI System partition for your linux drive. They can share the same partition.) Create a small bootloader partition using Apple's HFS+ filesystem that you will later bless as bootable in your OSx install. You'll then need a boot partition, root partition, and home partition. A swap partition is optional and I'm not going to go into it. 

* Upon booting from your Arch Linux live USB, run:

` cgdisk /dev/sda `

* Then partition your drive according to the table below. Make sure not to touch the first three partions which already exit. You are just creating the new ones:

<table>
  <tr>
    <th>Part # </th>
    <th></th>
    <th>Size </th>
    <th></th>
    <th>Partition type </th>
    <th></th>
    <th>Partition name</th>
  </tr>
  <tr>
    <td>1</td>
    <td></td>
    <td>200 Mib</td>
    <td></td>
    <td>EFI System </td>
    <td></td>
    <td>EFI system partition</td>
  </tr>
  <tr>
    <td>2</td>
    <td></td>
    <td>93 Gib </td> 
    <td></td>
    <td>Apple HFS+ </td>
    <td></td>
    <td>Macintosh HD</td>
  </tr>
  <tr>
    <td>3</td>
    <td></td>
    <td>700 MiB </td>
    <td></td>
    <td>Apple boot </td>
    <td></td>
    <td>Recovery HD</td>
  </tr>
  <tr>
    <td>4</td>
    <td></td>
    <td>128 MiB </td>
    <td></td>
    <td>Apple HFS+ </td>
    <td></td>
    <td>Linux Boot loader from Apple</td>
  </tr>
  <tr>
    <td>5</td>
    <td></td>
    <td>256 MiB </td>
    <td></td>
    <td>Linux filesystem </td>
    <td></td>
    <td>boot</td>
  </tr>
  <tr>
    <td>6</td>
    <td></td>
    <td>100 GiB </td>
    <td></td>
    <td>Linux filesystem</td>
    <td></td>
    <td>root</td>
  </tr>
  <tr>
    <td>7</td>
    <td></td>
    <td>250 GiB </td>
    <td></td>
    <td>Linux filesystem </td>
    <td></td>
    <td>home</td>
  </tr>
</table> 

### Network for installation

Just make sure you've had your ethernet plugged in. run ` ping google.com ` to make sure it's up, but you should be good out of the box. 

### Format partitions and install

` mkfs.ext2 /dev/sda5 `

` mkfs.ext4 /dev/sda6 `

` mkfs.ext4 /dev/sda7 `

` mount /dev/sda6 /mnt `

` mkdir /mnt/boot && mount /dev/sda5 /mnt/boot `

` mkdir /mnt/home && mount /dev/sda7 /mnt/home `

` pacstrap /mnt base base-devel `

` genfstab -p /mnt >> /mnt/etc/fstab `

If you storage device is SSD change your fstab file parameters:

{% codeblock [/mnt/etc/fstab] %}
nano /mnt/etc/fstab
/dev/sda5      /boot  ext2  defaults,relatime,stripe=4    0 2
/dev/sda6        /      ext4  defaults,noatime,discard,data=writeback   0 1
/dev/mapper/home /home  ext4  defaults,noatime,discard,data=ordered 0 2 
{% endcodeblock %}


### Configure

After everything is installed, set up your hostname, users, and locale. 

` arch-chroot /mnt /bin/bash `

` echo archer #or whatever you want to call it > /etc/hostname `

` ln -s /usr/share/zoneinfo/America/Buenos_Aires /etc/localtime `

` hwclock --systohc --utc `

` useradd -m -g users -G wheel -s /bin/bash codelitt && passwd codelitt `

` sudo pacman -S sudo `

` nano /etc/sudoers # uncomment wheel line `

To set up your locale, run:

` sudo nano /etc/locale.gen `

Uncomment the desired locals. For me this was:

` en_US.UTF-8 UTF-8 `

` en_CA.UTF-8 UTF-8 `

Generate these locales by running: 

`locale-gen `

` echo LANG=en_US.UTF8 > /etc/locale.conf`

` export LANG=en_US.UTF-8 `

You're going to modify your /etc/mkinitcpio.conf file to insert "keyboard" after "autodetect" in the HOOK section. Then run:

`mkinitcpio -p linux`

### Bootloader

This is probably one of the most confusing sections of the installation process particularly with Macbook's dual booting. The best way to do this is to boot directly from your Macbook's EFI boot loader so you'll need to create a boot.efi. 

 ` pacman -S grub-efi-x86_64 `

* Alter the /etc/default/grub with:

{% codeblock [/etc/default/grub] %}
GRUB_CMDLINE_LINUX_DEFAULT="quiet rootflags=data=writeback"`
{% endcodeblock %}

* Then you can generate the boot.efi with GRUB which you just installed. You'll want to put this on a USB device because you're going to be switching into OSx in a minute. 

`grub-mkconfig -o boot/grub/grub.cfg `
` grub-mkstandalone -o boot.efi -d usr/lib/grub/x86_64-efi -O x86_64-efi -C xz boot/grub/grub.cfg`

* This is going to create a file in which ever directory you are called boot.efi. Place it on a USB device. Check your devices then make a directory to mount your USB. Copy the boot.efi file onto your USB drive. 

` mkdir /mnt/usbdisk && mount /dev/sdb /mnt/usbdisk `
` cp boot.efi /mnt/usbdisk/ `

* Now you can exit chroot with `exit` and `umount` all of your filesystems from earlier. Reboot back into OSx.

* Launch disk utility, select the /dev/sda4. Select erase, select Mac OSx Journaled, and click Erase. This is where your grub2 image will live in. 

* Create directories and files in /dev/sda4.   

` cd /Volumes/disk0s4 `

` mkdir System mach_kernel `

` cd System `

` mkdir Library `

` cd Library `

` mkdir CoreServices `

` cd CoreServices `

` touch SystemVersion.plist`

* Copy over the boot.efi from the USB stick.

`cp /Volumes/usbstick/boot.efi boot.efi`

* Now you need to edit the SystemVersion.plist and add this:

{% codeblock %}
<xml version="1.0" encoding="utf-8"?>
<plist version="1.0">
<dict>
    <key>ProductBuildVersion</key>
    <string></string>
    <key>ProductName</key>
    <string>Linux</string>
    <key>ProductVersion</key>
    <string>Arch Linux</string>
</dict>
</plist> 
{% endcodeblock %}

* Now all you need to do is bless this partition to make it a bootable disk.

` sudo bless --device /dev/disk0s4 --setBoot `

* Now you can reboot and hold down your Option key. Select the second EFI disk on the list and hit enter to boot into Arch. Do the same but select the first to boot into OSx. 

### Xorg

The dreaded Xorg server. Luckily it goes pretty smoothly with the right drivers for your video card. You can change settings and play with your touchpad synaptics, backlighting, and everything else later. There is plenty of documentation out there. We're just going to get it running. Thsetup is for nvidia video card. If you're running an intel card for video then check the Wiki. 

` pacman -S xorg-server xorg-xinit xorg-server-utils xf86-input-synaptics nvidia acpid `
` systemctl enable acpid `
` nvidia-xconfig `

### Sound 

ALSA works out of the box with Macbooks. Install alsa-utils:

`pacman -S alsa-utils `

Then unmute the speakers. 

` alsamixer `

### Desktop

Alright. I don't use a desktop. I just use a WM called awesome. It's in the official repositories and it is kickass. That's what I'll show you how to install. 

* Get some fonts. 

`pacman -S ttf-dejavu ttf-google-fonts-hg #google fonts recommended but not necessary `

* Install awesome from the official repositories:

`pacman -S awesome `

* You'll want to create a config file because it doesn't come with one. 

` mkdir -p ~/.config/awesome/ `

* Copy a template file over to the directory you just created. 

` cp /etc/xdg/awesome/rc.lua ~/.config/awesome `

* There are many different themes available, but for now use the default theme. Change it later if you like. We'll copy the default theme over and then change the theme_path in the rc.lua config file for awesome. 

`cp -rf /usr/share/awesome/themes/default ~/.config/awesome/themes/default `
` nano ~/.config/awesome/rc.lua `

Change theme_path to:

`beautiful.init(awful.util.getdir("config") .. "/themes/default/theme.lua")`

* Before leaving change default terminalline to: 

`terminal = "xfce4-terminal" `

* Now it makes sense to install this terminal with:

`pacman -S xfce4-terminal `

* I don't use a login manager so add `exec awesome` to your startup script ~/.xinitrc

* Now from your tty you can startup your new WM with `startx`.

### WiFi

This part was probably the biggest pain in the ass of my entire install. The drivers don't load properly at first and everything is just better off when done manually instead of through the package manager. 

Thanks to Reddit user [jb270](http://www.reddit.com/user/jb270) on a thread I found recently and a bit of digging, I found a pretty fool proof way to get things rolling. 

* Download and extract the b43 firmware (https://aur.archlinux.org/packages/b43-firmware/) To extract use ` tar -zxf b43-firmware.xxx.xx `

* Change into the b43 folder and run this to download dependencies and install the package.

`makepkg -si` 

* Make sure to unload all known drivers before loading the new one. It can cause headache down the road if you don't:

` modprobe -r b43 bcma brcmsmac wl `

* Now load your new shiny b43 driver.

`modprobe b43 `

* Run dmesg and grep your wireless card to make sure everything worked fine. 

` dmesg | grep wlan0 `

If your interface is not called wlan0 then run `ip addr` to find your interface name. 

* Now we're going to disable the normal dhcpcd service and install probably the easiest to use Network Manager. 

` pacman -S networkmanager network-manager-applet gnome-keyring seahorse `

` systemctl stop dhcpcd@wlan0 `

` systemctl stop dhcpcd@eth0 `

` systemctl disable dhcpcd@wlan0 `

` systemctl disable dhcpcd@eth0 `

* Bring both your ip links down and back up with:

` ip link set eth0 down `

` ip link set wlan0 down `

` ip link set eth0 up `

` ip link set wlan0 up `

* You'll be using the gnome-keyring to save your wifi passwords and since you don't want the rest of the GNOME desktop, add this to your ~/.xinitrc file:

{% codeblock %}
# Start a D-Bus session
source /etc/X11/xinit/xinitrc.d/30-dbus
# Start GNOME Keyring
eval $(/usr/bin/gnome-keyring-daemon --start --components=gpg,pkcs11,secrets,ssh)
# You probably need to do this too:
export SSH_AUTH_SOCK
export GPG_AGENT_INFO
export GNOME_KEYRING_CONTROL
export GNOME_KEYRING_PID`
{% endcodeblock %}

* Now you just need to enable NetworkManager dameons and start the nm-applet

` systemctl enable NetworkManager `

` systemctl start NetworkManager `

` nm-applet `

The applet should appear in the top right hand corner of your awesome windows manager. The rest should be pretty self-explanatory. 

The rest you should be able to figure out, but commment here or ping me on [Twitter @codelitt](http://twitter.com/codelitt)

 
