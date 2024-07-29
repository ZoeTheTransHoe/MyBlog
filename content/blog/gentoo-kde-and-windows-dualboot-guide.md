+++
title = "How I Setup Gentoo KDE/Windows 10 Dualboot."
date = 2024-05-28
+++

I recently got a new SSD for my Elitebook 820. While it did run Debian 12 before on its archaic 320GB Spinning HDD, this new SSD should modernize it and make it.
So whats more taxing for this computer and me than running a bloated OS and an OS you have to compile everything for (including sanity)? Using both at the same time! 

This blog will entail the steps I took to dualboot Windows 10 and Gentoo Linux, using the KDE Desktop Environment, on Systemd with Flatpak, Wayland and Pipewire/Portals support.

<img class="largeImages" src="../../../img/Articles/Blog3/gentoo-windows-dualboot.png">

<h1> The Specimen </h1>
For this demonstration, I will be installing Gentoo/Windows onto my HP Elitebook G820. The Specs Are:

<ul>
    <li>CPU: Intel i5-4300U </li>
    <li>RAM: 8GB DDR3</li>
    <li>GPU: Intel HD 4400</li>
    <li>Drive Space: 120GB SSD</li>
</ul>

<h1> Preparations </h1>
To start, you will need 3 things downloaded from another computer.
<ol>
    <li> A Windows 10 ISO. This can be obtained from <a href="https://www.microsoft.com/en-us/software-download/windows10%20"> Microsoft's Offical Website. </a> </li>
    <li> A Live Linux USB for installing Gentoo from. While you are free to use other Linux Distros, since Gentoo can be bootstrapped from pretty much any exisiting Linux Distro, I will be using the official Gentoo Minimal USB, so I only have a CLI interface to reduce overhead - important for when we are full speed ahead compiling software later! <a href="https://distfiles.gentoo.org/releases/amd64/autobuilds/20240526T163557Z/install-amd64-minimal-20240526T163557Z.iso"> You can grab the Gentoo Minimal Live USB Here.</a></li>
    <li> A USB Stick, and <a href="https://sourceforge.net/projects/ventoy/files/v1.0.98/">Ventoy</a> for putting both the ISO files onto one USB Stick.</li>
</ol>

Once you have obtained all of these files, we can get started!

<h1> Getting Setup </h1>

The next steps is getting Ventoy setup. This will slightly difference on if you are on Windows Or Linux. If you already know how to use Ventoy you can skip to here.

<h2> Linux </h2>
<ol>
    <li> Download the Linux tar.gz from <a class="https://sourceforge.net/projects/ventoy/files/v1.0.98/">here</a></li>
    <li> Extract the file once it is done downloading, and enter the extracted file created. It should look something like this.</li>
    <img class="images" src="../../../img/Articles/Blog3/Screenshot_20240527_174730.png">
    <li> Run the file named "VentoyGUI.x86_64". You may need to make it executable, which can be done by right clicking on it and checking its properties if you are using GNOME or KDE, or running the following command:</li>
    <div class="codePadding"> <code>sudo chmod +x ./VentoyGUI.x86_64</code> </div>
    <li> In the following window, select your USB stick in the button labelled 1, and click "Install" on the button labelled 2. Press OK on the following two windows that popup.</li>
    <img class="images" src="../../../img/Articles/Blog3/Screenshot_20240527_180037.png">
    <li> Now Ventoy Is Installed, navigate to the new partition named "Ventoy", and place both the Windows and Gentoo ISOs in this partition. Once they are added, remove the USB drive from your computer and place it into the system you wish to install the two OSes onto. </li>
    <img class="images" src="../../../img/Articles/Blog3/Screenshot_20240527_180450.png">
    <img class="images" src="../../../img/Articles/Blog3/Screenshot_20240527_180455.png">
</ol>

<h2> Windows </h2>
<ol>
    <li> Download the Windows zip from <a class="https://sourceforge.net/projects/ventoy/files/v1.0.98/">here.</a></li>
    <li> Extract the file once it is done downloading, and enter the extracted file created. It should look something like this.</li>
    <img class="images" src="../../../img/Articles/Blog3/Untitled.png">
    <li> Run the file named "Ventoy2Disk" . UAC will prompt you if you want to run the app.</li>
    <li> In the following window, select your USB stick in the button list at the top and click "Install" at the bottom. Press OK on the following two windows that popup.</li>
    <img class="images" src="../../../img/Articles/Blog3/8.PNG">
    <li> Now Ventoy Is Installed, navigate to the new partition named "Ventoy", which should be your D:\ or similar, and then place both the Windows and Gentoo ISOs in this partiton. Once they are added, remove the USB drive from your computer and place it into the system you wish to install the two OSes onto. </li>
    <img class="images" src="../../../img/Articles/Blog3/9.PNG">
</ol>

Once Ventoy Is Installed, Boot up the target machine and boot into the Ventoy USB using your computers boot menu. Navigate down to the Windows ISO using the arrow keys and press enter twice. Now go ahead and get Windows 10 installed. Do not attempt to rush ahead and install Gentoo first, it is much less hassle and headache to install Windows 10 first then install Gentoo (this really goes for dualbooting any Windows and Linux).

If you are reading this, I am guessing you know how to install Windows 10. Its not a pleasant installer but its easy all things considered. 

Once Windows 10 is installed, make sure to disable fast boot as it can mess with booting our other Linux install. This can be done by:

<ol>
    <li> Search For "Edit Power Plan" in the search bar. </li>
    <img class="images" src="../../../img/Articles/Blog3/Capture.PNG">
    <li> Go To "Power Options" in the top bar </li>
    <li> Click "Choose what the power buttons do" </li>
    <img class="images" src="../../../img/Articles/Blog3/2.PNG">
    <li> Click on "Change Settings that are currently unavailable </li>
    <img class="images" src="../../../img/Articles/Blog3/3.PNG">
    <li> Untick "Turn on fast start-up" and Save Changes</li>
</ol>

Before we head into Gentoo, we need to shrink Windows 10s partition so we have room to install Gentoo onto.
<ol>
    <li> Search For "Disk Management" in the search bar.</li>
    <img class="images" src="../../../img/Articles/Blog3/4.PNG">
    <li> Left-Click on your Windows 10 C: Partition, and press shrink volume </li>
    <img class="images" src="../../../img/Articles/Blog3/5.PNG">
    <li> Enter in your desired size in MBs of your Gentoo Partition.</li>
    <li> Once shrunk, you should have your C drive and some free space - it should look something like the below:</li>
    <img class="images" src="../../../img/Articles/Blog3/6.PNG">
</ol>

Now, Windows is setup and configured correctly, its time for the fun part!

<h1> Gentoo Time! </h1>

Reboot your machine back into Ventoy, and load into the Gentoo Live USB. The rest of this guide will be written in mind that you are using the Gentoo Live USB.

When prompted, enter in the keycode for your specific keymap (for me, that is uk), and wait for Gentoo to load up and you get into a shell prompt.

<h2> Set Up Internet Access </h2>
If you are on ethernet, you can skip this step. However, for my fellow wifi enthusiast, follow the following steps to get connected to your wifi network.

<ol>
    <li> Run the following command to find your wifi card.
    <div class="codePadding"> <code>livecd ~ # iwconfig</code></div> 
    The command should output an interface name that is followed by <code>IEEE 802.11</code> - thats your wireless interface, write it down.</li>
    <li> Run the following command to find the ESSIDs (network names) the wireless card can run, replacing "YOUR_WIFI_INTERFACE_HERE" with the interface name obtained in the previous step.
    <div class="codePadding"><code>livecd ~ #  iwlist YOUR_WIFI_INTERFACE_HERE scan | grep ESSID</code></div>
    You should be able to see the network name of the wifi network you wish to connect too.</li>
    <li> Add your wifi networks credentials using this command, replacing YOUR_WIFI_NETWORK with your wifi network's ESSID, and YOUR_WIFI_PASSWORD with the password of your network.
    <div class="codePadding"><code>livecd ~ # wpa_passphrase YOUR_WIFI_NETWORK YOUR_WIFI_PASSWORD | tee /etc/wpa_supplicant.conf</code></div></li>
    <li> Connect to your wifi network using the following command replacing "YOUR_WIFI_INTERFACE_HERE" with your interface name.
     <div class="codePadding"><code>livecd ~ # wpa_supplicant -B -c /etc/wpa_supplicant.conf -i YOUR_WIFI_INTERFACE_HERE</code></div> It should spit out <div class="codePadding"><code>Successfully initialized wpa_supplicant</code></div></li>
    <li> Done! Run a Ping command to ensure you can access the outside world.  <div class="codePadding"><code>livecd ~ # ping google.com</code></div> </li>
</ol>

Now, lets jump into the main cheese and biscuits. Grab some cheese and biscuits actually, and get comfortable, this is gonna be a long process. 

Most of this will be ripped from the <a src="https://wiki.gentoo.org/wiki/Handbook:AMD64"> Gentoo Wiki.</a>. However, this guide will be a more focused, full guide to get from TTY > Full KDE Desktop will all the trimmings, while dualbooting Windows, compared to the Gentoos wiki being more generic. You are free to use either wikis, or both.

Lets get started!

<h2> Prepare Disks! </h2>

Run the following to get the storage devices detected by the system - make double sure you pick out the drive that corresponds to the drive you want to install Gentoo onto!
<div class="codePadding"><code>livecd ~ # lsblk</code></div> 
This drive will usually be /dev/sda or /dev/nvme01. Run the following to start the partitioning tool, replacing MY_DRIVE with the drive you wish to install Gentoo Linux onto. 

<div class="codePadding"><code>livecd ~ # cfdisk MY_DRIVE</code></div> </li>

Create Two New Partitions in the empty space we created in Windows before hand.:
    <ul>
        <li> Swap Partition, about the same size as RAM.</li>
        <li> Root Partition, rest of the disk</li>
    </ul>

Make the swap partition a Swap type, and write and quit cfdisk.Once back in the terminal prompt, run lsblk again to identify which partition is the swap and root, and note this down - it'll come in handy a lot later! 
<div class="codePadding"><code>livecd ~ # lsblk</code></div></li>

Run the following three commands to format the two new parititons and setup swap 
<div class="codePadding"><code>livecd ~ # mkfs.ext4 /dev/YOUR_ROOT</code></div>
<div class="codePadding"><code>livecd ~ # mkswap /dev/YOUR_SWAP</code></div>
<div class="codePadding"><code>livecd ~ # swapon /dev/YOUR_SWAP</code></div></li>

Mount the root partition using the following command
<div class="codePadding"><code>livecd ~ # mount /dev/YOUR_ROOT /mnt/gentoo</code></div></li>

Change into the directory of your new mounted root
<div class="codePadding"><code>livecd ~ # cd /mnt/gentoo</code></div></li>

<h2> Installing Stage3 </h2>

Now we can install the Stage3 tarball, that will contain the foundation of our new Gentoo install. There are many foundations - those who want no desktop, or looking to run a desktop, those wanting to use a different init system or CPU architectures e.t.c. For this guide, we want a desktop on systemd and AMD64. To get the latest tarball, we can go over to [https://www.gentoo.org/downloads/](https://www.gentoo.org/downloads/), and copy the link on "Stage3 desktop profile | systemd."

<img class="images" src="../../../img/Articles/Blog3/Screenshot_20240528_200200.png">

Back in our terminal, we will paste in the following command to grab us this tarball, where STAGE3_DOWNLOAD_LINK is the link we copied from earlier.
<div class="codePadding"><code>livecd ~ # wget STAGE3_DOWNLOAD_LINK</code></div>

Now, we could just assume it all downloaded properly - but lets be safe rather than sorry! Run the following command to get the SHA512 signature and compare it to the tar.xz we just downloaded. 

<div class="codePadding"><code>livecd ~ # wget STAGE3_DOWNLOAD_LINK.sha256</code></div>
<div class="codePadding"><code>livecd ~ # sha256sum --check YOUR_TAR_FILE.sha256</code></div>

You should get a:
<div class="codePadding"><code>YOUR_TAR_FILE : OK</code></div>

Now the fun (and slightly long) part - copy in the next command to extract the stage3 file.

<div class="codePadding"><code>livecd ~ # tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner</code></div>
    

<h2> Configuring Compilation </h2>

Now, we can start editing the big important make.conf file - the file that dictate how the software we download later will be compiled. Make sure you take care editing this file! 
Open up the file using whatever text editor you desire- I will use Vi, but if you hate God you can use Emacs or Nano.

<div class="codePadding"><code>livecd ~ # vi /mnt/gentoo/etc/portage/make.conf</code></div>

In COMMON_FLAGS, add the following:

<div class="codePadding"><code>COMMON_FLAGS="-march=native -O2 -pipe"</code></div>

MAKEOPTS will define how many threads will be used to compile our software - a good rule of thumb is for the number of jobs is either the number of threads in your PC or the amount of memory in GBs you have divide by 2, whichever is lower.  The load-average will be the job count + 2. 

On my machine, I will have 4 threads and 8GBs, so will make my job count 4 and my load count 6, for example.

Add the following line to your make.conf, substituting 4 and 6 with the appropriate number of jobs/load averages for your machine:
<div class="codePadding"><code>MAKEOPTS="-j4 -l6"</code></div>

Save and exit the file. Lets move onto the next step!

<h2> Entering New System </h2>

Now we have the configuration for compiling our software, we can enter our new environment. To do so, we need to copy over our DNS info, then chroot into the new environment. To do so, enter in the following two commands.

<div class="codePadding"><code>livecd ~ # cp --dereference /etc/resolv.conf /mnt/gentoo/etc/</code></div>

**NOTE THE FOLLOWING COMMAND WILL NOT WORK IF YOU ARE NOT USING OFFICIAL GENTOO INSTALL MEDIA - PLEASE LOOK [HERE](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base) IF YOU ARE NOT DOING SO**
<div class="codePadding"><code>livecd ~ # arch-chroot /mnt/gentoo</code></div>

You should now see your prompt change to 
<div class="codePadding"><code>livecd / #</code></div>

To initalize entering in the new environment, enter in the next 2 commands.

<div class="codePadding"><code>livecd / # source /etc/profile  </code></div>
<div class="codePadding"><code>livecd / # export PS1="(chrOwOt) ${PS1}" </code></div>

<h2> Preparing Bootloader </h2>

These steps will be different if your system is either UEFI (has an EFI partition) or Legacy BIOS (doesn't have an EFI partition). My machine is so ancient it uses Legacy BIOS, but for conviences sake I will leave both sets of commands needed to mount the boot

<h3> UEFI Systems </h3>
<div class="codePadding"><code>livecd / # mkdir /efi  </code></div>
<div class="codePadding"><code>livecd / # mount /dev/EFI_PARTITON /efi  </code></div>

<h3> Legacy BIOS Systems </h3>
No Additional Steps Needed at this point.

<h2> Preparing Portage </h2>
Portage will be our package management system, managing compilation, updating and removing software from our system. 

The first thing we shall do is sync with portage, to make sure the software repository is up to date. This can be done by:
<div class="codePadding"><code>livecd / # emerge-webrsync  </code></div>
<div class="codePadding"><code>livecd / # emerge --sync  </code></div>

Now we need to pick our profile - i.e, some extra building blocks to help build the system we want in the end. Since we want to use KDE Plasma with systemd on a desktop, we can pick option 28 - amd64/23.0/desktop/plasma/systemd

<div class="codePadding"><code>livecd / # eselect profile set 28  </code></div>

Now for the fun part - configuring USE! These are the flags that define which features we do and do not want for the software we are downloading. 
To set the USE flags, open the make.conf file again.

<div class="codePadding"><code>livecd / # vi /etc/portage/make.conf</code></div>

For our KDE/Wayland/Pipewire/Systemd setup, copy in the following USE flags - you are however, free to tweak them as you see fit!

<div class="codePadding"><code>USE="-gtk -gnome -cdr -dvd -browser-integration -crash-handler -firewall -handbook -sdk -thunderbolt -wacom -wallpapers -webengine -ieee1394 -jack-client accessibility colord bluetooth qt5 kde alsa crypt cups desktop-portal discover display-manager flatpak grub kwallet legacy-systray networkmanager plymouth pulseaudio sddm smart systemd X xwayland dbus ffmpeg sound-server wifi policykit jack-sdk pipewire-alsa extra dracut mount python"</code></div>

Now we need to add the CPU/GPUs we will be using to our Portage configuration.

For our CPUs, run the following commands to download the program that will parse and then add our supported CPU Flags.
<div class="codePadding"><code>livecd / # emerge --ask --oneshot cpuid2cpuflags</code></div>
<div class="codePadding"><code>livecd / # echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpu-flags</code></div>

For our GPUS, this will depend on if you use Intel, AMD, or NVIDIA. I am using Intel Haswell (Gen 7) iGPU on this machine, for AMD see [here](https://wiki.gentoo.org/wiki/AMDGPU), and for Nvidia (FOSS) see [here](https://wiki.gentoo.org/wiki/Nouveau), and for Nvidia (Proprietary) see [here](https://wiki.gentoo.org/wiki/NVIDIA).

In any case, add the following line in make.conf, with your specific video card flags.
<div class="codePadding"><code>VIDEO_CARDS="intel"</code></div>

Next, we will add to out make.conf, we need to add the software liscenses we want to use. Since the FSF dont have a gun to my head, I will add it to allow any license in the make.conf.

<div class="codePadding"><code>ACCEPT_LICENSE="*"</code></div>

Now, lets update emerge to make use of all of these lovely new flags. This will take a long time, especially since - it took me around 8 hours on my shitty Elitebook!

<div class="codePadding"><code>livecd / # emerge --ask --verbose --update --deep --newuse @world</code></div>

Then remove unneeded dependencies by running the following
<div class="codePadding"><code>livecd / # emerge --depclean </code></div>

<h2> Configuring Locales / Keymaps </h2>

First, lets configure which time zone we are in. First, lets check the timezone file to see what options we have.

Generally, the timezone for you will be your capital city in your country, in the continent you live in. 

<div class="codePadding"><code>livecd / # ls -l /usr/share/zoneinfo</code></div>

Since I live in the UK, it will therefore be in <code>Europe/London</code>. To tell systemd to use this, make a symbolic link between this timezone and the file it looks for, for localetime, replacing YOUR_TIME_ZONE with your specific time zone. 

<div class="codePadding"><code>livecd / # ln -sf /usr/share/zoneinfo/YOUR_TIME_ZONE /etc/localtime</code></div>

To configure locales, first check the below file for the possible locales that can be used 
<div class="codePadding"><code>livecd / # vim /usr/share/i18n/SUPPORTED</code></div> 
Once you have found the locale you wish to use, close that file then open up the locale generation file, and add the locales you wish to use.

<div class="codePadding"><code>livecd / # vim /etc/locale.gen</code></div>

<div class="codePadding"><code>
en_GB ISO-8859-1

en_GB.UTF-8 UTF-8</code></div>

Save and exit that file, and run the command to generate the new locales.

<div class="codePadding"><code>livecd / # locale-gen</code></div>

Now update your new locales.

<div class="codePadding"><code>livecd / # env-update && source /etc/profile && export PS1="(chrOwOt) ${PS1}"</code></div>

<h2> Installing a Kernel. </h2>

While we have a lot of useful tools so far, we are missing the most important of all - the kernel! While we could configure our own kernel and you are more than welcome to do so, for this guide I will fall back and use the binary kernel for the sake of time.

First, download the firmware needed by the kernel:
<div class="codePadding"><code>livecd / # emerge --ask linux-firmware sof-firmware</code></div>

For Intel CPUs, download microcode updates too:
<div class="codePadding"><code>livecd / # emerge --ask intel-microcode</code></div>

Now, lets install the Kernel
<div class="codePadding"><code>livecd / # emerge --ask gentoo-kernel-bin</code></div>

<h2>  Configuring filesystems. </h2>

While we could manually edit our own fstab (the partitions Linux will automount while booting) ourselves, we could nab the [tool](https://github.com/glacion/genfstab) Arch Linux uses to do this automatically for its install guide.

First, download git so we can download the fstab generation script from GitHub
<div class="codePadding"><code>livecd / # emerge --ask dev-vcs/git</code></div>

Now clone the git repo that contains genfstab
<div class="codePadding"><code>livecd / # cd tmp</code></div>
<div class="codePadding"><code>livecd / # git clone https://github.com/glacion/genfstab.git</code></div>

<div class="codePadding"><code>livecd / # cd genfstab</code></div>
Change to a different TTY (Ctrl + Alt + F2) and the following command
<div class="codePadding"><code>livecd ~ # /mnt/gentoo/tmp/genfstab/genfstab -U /mnt/gentoo >> /mnt/gentoo/etc/fstab</code></div>

Switch back to the TTY in our new install chroot (Ctrl + Alt + F1)

<h2>  Setting System information </h2>

Since we are running systemd, we can run the following commands to setup systemd for our system.

<div class="codePadding"><code>livecd / # systemd-machine-id-setup</code></div>
<div class="codePadding"><code>livecd / # systemd-firstboot --prompt</code></div>
<div class="codePadding"><code>livecd / # systemctl preset-all --preset-mode=enable-only</code></div>

To make using the terminal slightly more tolerable later, we can download auto-completion tools for our shell, bash.

<div class="codePadding"><code>livecd / # emerge --ask bash-completion</code></div>

To enable time sync using systemd, run the following command
<div class="codePadding"><code>livecd / # systemctl enable systemd-timesyncd.service</code></div>

Other useful tools we can download now includes udev-rules for dealing with some storage device types, and wpa_supplicant so we have a fallback to connect to the interent later.

<div class="codePadding"><code>livecd / # emerge --ask iw wpa_supplicant io-scheduler-udev-rules</code></div>

<h2>  Configuring the bootloader </h2>

This step will be slightly different deepening on if you are using BIOS or UEFI systems. 

If you are on UEFI, run the following command (BIOS Chads do not need to do this.)

<div class="codePadding"><code>livecd / # echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf</code></div>

Both UEFI and BIOS users, run the following command to grab grub and os-prober.

<div class="codePadding"><code>livecd / # emerge --ask grub os-prober
</code></div>


Now run the following commands (depending on if you are on UEFI or BIOS)

<h3> UEFI Systems </h3>
<div class="codePadding"><code>livecd / # grub-install --efi-directory=/efi
</code></div>

<h3> BIOS Systems </h3>

<div class="codePadding"><code>livecd / # grub-install /dev/YOUR_DRIVE</code></div>

By default, GRUB will not search for other partitions - including our Windows 10 partition. To allow it to do so, open up the following file
<div class="codePadding"><code>livecd / # vim /etc/default/grubprober</code></div>

And add the following line at the end <div class="codePadding"><code>GRUB_DISABLE_OS_PROBER=false </code></div>
 
Now we can tell grub to configure itself, includin which  partitions it lists at boot time.

<div class="codePadding"><code>livecd / # grub-mkconfig -o /boot/grub/grub.cfg </code></div>

<h2>  Daily Use Administration </h2>

Now the fun part - adding your user and adding in the software we will use on a desktop - that means KDE, Pipewire e.t.c!

First lets add a normal user and give them a password. You can replace zelda with whatever name you desire.

<div class="codePadding"><code>livecd / # useradd -m -G users,wheel,audio,video -s /bin/bash zelda</code></div>
<div class="codePadding"><code>livecd / # passwd zelda</code></div>

As a security measure, lets disable root login.
<div class="codePadding"><code>livecd / # passwd -dl root</code></div> 

Now, to let our user perform access that needs super user access, lets install sudo and configure it to allow our user to run commands as super user.

<div class="codePadding"><code>livecd / # emerge --ask sudo</code></div> 

Quick aside - if you are using vim/emacs, you might notice sudoedit doesn't work and complains about not having nano available
eselect editor list
eselect editor set 1
. /etc/profile

Open up the sudo configuration file
<div class="codePadding"><code>livecd / # sudoedit /etc/sudoers </code></div>
Uncomment this line
<div class="codePadding"><code># %wheel ALL=(ALL:ALL) ALL</code></div>  

Save and exit this file.Now for the fun part - installing our audio system and Desktop environment. Be warned, this will take a very long time to compile, so hang in there, and grab a beverage while it does so.

Run the following command to start the big merge:

<div class="codePadding"><code>livecd / # emerge --ask pipewire wireplumber plasma-meta konsole kde-apps/dolphin </code></div> 

Now, to exit the live medium, run the following commands to exit the chroot, unmount all partitions and reboot the computer
<div class="codePadding"><code>livecd / # exit</code></div> 
<div class="codePadding"><code>livecd ~ # cd</code></div> 
<div class="codePadding"><code>livecd ~ # umount -l /mnt/gentoo/dev{/shm,/pts,} </code></div> 
<div class="codePadding"><code>livecd ~ # umount -R /mnt/gentoo </code></div> 
<div class="codePadding"><code>livecd ~ # reboot </code></div> 

Remove USB stick while the PC is rebooting - you should now be greeted by GRUB!

Reboot into both OSes, then once in Gentoo log in with your users credentials and run the following command to enable SDDM

<div class="codePadding"><code>zoey@zelda ~ $ sudo systemctl enable --now sddm </code></div> 
Once in KDE, you may notice there is no sound, bluetooth or networking. Dont panic, just run the following commands to enable NetworkManager, Bluetooth and Pipewire
<div class="codePadding"><code>zoey@zelda ~ $ sudo systemctl enable --now -user wireplumber pipewire pipewire-pulse </code></div> 
<div class="codePadding"><code>zoey@zelda ~ $ sudo systemctl enable --now NetworkManager bluetooth</code></div> 

Finally, add Flathub to your flatpak repos so you can use flatpaks.
<div class="codePadding"><code>zoey@zelda ~ $ flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo</code></div> 

Done!
<img class="largeImages" src="../../../img/Articles/Blog3/gentoo_desktop.png">

<h1>  Extras For Gentoo </h1>

<h2>  Install Hyfetch :3 </h2>
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask hyfetch</code></div> 

<h2>  Use Samba Shares </h2>
Open the make.conf file again, and add the samba USE flag.
<div class="codePadding"><code>zoey@zelda ~ $ sudo vim /etc/portage/make.conf</code></div>

<div class="codePadding"><code>USE="... samba"</code></div>
Save and exit the file, then run the following commands.
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge -avtDUu @world</code></div> 
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask kio-extras</code></div> 

<h2>  Install Spectacle For Screenshots </h2>
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask spectacle</code></div> 

<h2>  Install MPV and JuK for multimedia.</h2>
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask mpv juk</code></div> 

<h2>  Install Gwenview or qView for looking at photos.</h2>

For Gwenview:
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask gwenview</code></div>
For qView:
<div class="codePadding"><code>zoey@zelda ~ $ flatpak install qView </code></div> 

<h2>Install Steam</h2>

This is somewhat involved. The easy route is just use the [Flatpak](https://flathub.org/apps/com.valvesoftware.Steam), but depeneding on your system it could have issuses of its own.

To get native Steam, we need to add an overlay (a seperate repo with its own packages.) Run the below command

<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask --noreplace app-eselect/eselect-repository</code></div>

Enable the steam repo using the following command.
<div class="codePadding"><code>zoey@zelda ~ $ sudo eselect repository enable steam-overlay</code></div>

Then sync the repos.
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --sync</code></div>

Steam needs its own special USE flags to allow it to use 32-bit libraries. Open up the following package 

<div class="codePadding"><code>zoey@zelda ~ $ sudo vim /etc/portage/package.use/steam</code></div>

And copy and paste in the following.

<div class="codePadding"><code>
x11-libs/libX11  abi_x86_32

x11-libs/libXau  abi_x86_32

x11-libs/libxcb  abi_x86_32

x11-libs/libXdmcp  abi_x86_32

virtual/opengl  abi_x86_32

media-libs/mesa  abi_x86_32

dev-libs/expat  abi_x86_32

media-libs/libglvnd  abi_x86_32

sys-libs/zlib  abi_x86_32

x11-libs/libdrm  abi_x86_32

x11-libs/libxshmfence  abi_x86_32

x11-libs/libXext  abi_x86_32

x11-libs/libXxf86vm  abi_x86_32

x11-libs/libXfixes  abi_x86_32

app-arch/zstd  abi_x86_32

sys-devel/llvm  abi_x86_32

x11-libs/libXrandr  abi_x86_32

x11-libs/libXrender  abi_x86_32

dev-libs/libffi  abi_x86_32

sys-libs/ncurses  abi_x86_32

dev-libs/libxml2  abi_x86_32

dev-libs/icu  abi_x86_32

sys-libs/gpm  abi_x86_32

virtual/libelf  abi_x86_32

dev-libs/elfutils  abi_x86_32

app-arch/bzip2  abi_x86_32

dev-libs/nspr  abi_x86_32

dev-libs/nss  abi_x86_32

net-libs/libndp  abi_x86_32

x11-libs/extest abi_x86_32

dev-libs/libevdev abi_x86_32

dev-libs/wayland abi_x86_32

virtual/rust abi_x86_32

dev-lang/rust-bin abi_x86_32

x11-libs/libpciaccess abi_x86_32

sys-devel/clang abi_x86_32

media-libs/fontconfig abi_x86_32

sys-libs/libudev-compat abi_x86_32

media-libs/libpulse abi_x86_32

media-libs/libsndfile abi_x86_32

net-libs/libasyncns abi_x86_32

sys-apps/dbus abi_x86_32

dev-libs/glib abi_x86_32

dev-libs/libpcre2 abi_x86_32

sys-apps/util-linux abi_x86_32

media-libs/flac abi_x86_32

media-libs/libogg abi_x86_32

media-libs/libvorbis abi_x86_32

media-libs/opus abi_x86_32

media-sound/lame abi_x86_32

media-sound/mpg123-base abi_x86_32

media-libs/freetype abi_x86_32

media-libs/libpng abi_x86_32

virtual/libintl abi_x86_32

virtual/libudev abi_x86_32

sys-apps/systemd-utils abi_x86_32

sys-libs/libcap abi_x86_32

sys-libs/pam abi_x86_32

virtual/libiconv abi_x86_32

x11-libs/xcb-util-keysyms abi_x86_32

dev-libs/libgpg-error abi_x86_32

app-arch/lz4 abi_x86_32

dev-libs/libgcrypt abi_x86_32

sys-apps/systemd abi_x86_32
</code></div>

Save and exit. 

To accept installing the steam package, open the following file.

<div class="codePadding"><code>zoey@zelda ~ $ sudo vim /etc/portage/package.accept_keywords/steam</code></div>

<div class="codePadding"><code>*/*::steam-overlay
games-util/game-device-udev-rules
sys-libs/libudev-compat</code></div>

Save and exit.

We need to re-emerge ncurses with gpm support.
<div class="codePadding"><code>zoey@zelda ~ $ sudo USE="-gpm" emerge --ask --oneshot sys-libs/ncurses </code></div>


Install Steam:
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask games-util/steam-launcher</code></div>

Re-emerge gpm and ncurses
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask --oneshot sys-libs/ncurses gpm</code></div>

<h2>Resource Monitoring</h2>

To install HTOP:
<div class="codePadding"><code>zoey@zelda ~ $ sudo emerge --ask htop</code></div>
