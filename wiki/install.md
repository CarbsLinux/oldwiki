Installation Instructions
=========================

This post will guide you step-by-step in order to install
Carbs Linux. It is not complete yet.


Downloading and Getting Ready to Chroot
---------------------------------------

Installing Carbs Linux is the same as installing most 
rootfs distributions, such as Gentoo, or Void Linux.


### Requirements

You simply need a Live Linux ISO. Gentoo would be the best
as programs you will require will be already there. Get the
ISO, boot it and set up your network.

**Required Programs:**

* tar
* wget
* xz

Rest of the instructions assume that you have set all of these
up, and will continue on.


### Download


First, we need to download the rootfs tarball. In your terminal
type

    $ wget https://carbslinux.org/releases/carbs-rootfs-20191210.tar.xz


### Check the integrity of the tarball

While optional, it is highly recommended to check the integrity
of the tarball.

    $ wget https://carbslinux.org/releases/carbs-rootfs-20191210.tar.xz.sha256sum
    $ sha256sum -c carbs-rootfs-20191210.tar.xz.sha256sum


### Check the signatures

While optional, it is highly recommended to check the signature 
of the tarball

    $ wget https://carbslinux.org/releases/carbs-rootfs-20191210.tar.xz.sig
    
    Import the public key of release@carbslinux.org
    
    $ gpg --recv-keys FF484BDFEFCEF8FF
    
    $ gpg --verify carbs-rootfs-20191210.tar.xz.sig

### Download Kiss Linux Chroot Helper

    $ wget https://dl.getkiss.org/kiss-chroot
    
    Make the script executable
    
    $ chmod +x kiss-chroot

### Extract the rootfs tarball

You need to extract the tarball to your mounted partition

    $ tar xvpf carbs-rootfs-20191210.tar.xz -C /mnt

Chroot
------
Chroot into Carbs Linux

    $ ./kiss-chroot /mnt

### Enable repository signing (Highly Recommended)

This step is optional, but very highly recommended 
(I cannot stress out the amount I am recommending this). 
This ensures you are receiving updates from Carbs Linux 
and not some other source.

**Install GnuPG**

It is also a great time to learn how to use your new 
package manager, KISS! You simply build and install
the package

    $ kiss build gnupg1
    $ kiss install gnupg1

**Import my key**


    Import my public key (cem@ckyln.com)
    
    $ gpg --recv-key 4356EE08A364CE09
    
    Create a .gnupg directory if it doesn't exist
    
    $ mkdir -p /root/.gnupg
    
    Trust my public key
    
    $ echo trusted-key 0x4356ee08a364ce09 >> /root/.gnupg/gpg.conf

**Enable signature verification**


Git has a built in signature verification feature.
You can go to the repository directory and enable it

    $ cd /var/db/kiss/repo
    $ git config merge.verifySignatures true

### Installing Important Tools

Since Carbs gives you one of the most minimal base,
you might need to apply some add-ons to your system.

**Update packages**

It might be a good idea to update your packages before
installing new packages. This ensure compatibility.

    $ kiss update

**Filesystems**


Currently, e2fsprogs and dosfstools are supported,
as they are the most-used filesystems.

    Support for ext2, ext3, and ex4
    
    $ kiss build e2fsprogs
    $ kiss install e2fsprogs
    
    Support for dosfstools
    
    $ kiss build dosfstools
    $ kiss install dosfstools

**Device management**

    $ kiss build eudev
    $ kiss install eudev

**Wireless**

**wpa_supplicant**

    $ kiss build wpa_supplicant
    $ kiss install wpa_supplicant

**eiwd (WIP)**

    $ kiss build eiwd
    $ kiss install eiwd

**dhcpcd**

    $ kiss build dhcpcd
    $ kiss install dhcpcd

**Shells**

Carbs Linux comes preinstalled with busybox ash, but other shells
can be found on the repository

    Install bash
    
    $ kiss build bash
    $ kiss install bash
    
    Install zsh
    
    $ kiss build zsh
    $ kiss install zsh

    Install Plan9 rc-shell

    $ kiss build 9base
    $ kiss install 9base

### Install the Kernel


Next step is installing the kernel, which might sound as a challenge
you should download and extract the kernel tarball first.

    $ wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.5.tar.xz
    
    Let's move the sources to /usr/src
    
    $ mv linux-5.5.tar.xz /usr/src
    $ tar xf linux-5.5.tar.xz
    $ cd linux-5.5

**Configure the Kernel**

You can check <https://wiki.gentoo.org/wiki/Kernel/Configuration>
to learn more about kernel configuration. Overall, [Gentoo Wiki](https://wiki.gentoo.org)
is a good place to learn about kernel configuration related to
your hardware.

**Build and install the kernel**

The rest of it is simple, build the kernel with
`make` and install it with `make install`

### Bootloader

The default bootloader is GRUB (GRand Unified Bootloader).

Build and install GRUB

    $ kiss b grub
    $ kiss i grub
    
    Install efibootmgr as well if your
    system requires it
    
    $ kiss b efibootmgr
    $ kiss i efibootmgr


**Setup GRUB**

    For BIOS:
    $ grub-install --target=i386-pc /dev/sdX
    
    For UEFI:
    $ grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=CarbsLinux
    
    $ grub-mkconfig -o /boot/grub/grub.cfg


### Install init

Currently, default init for Carbs Linux is `sinit`, but
busybox-init is also supported. You can also change this yourself.

To install init, you have to

    $ kiss b sinit
    $ kiss i sinit

This will also fetch busybox-init as its dependency.


### Enabling other repositories

There are other repositories out there that might be of your
interest. The Official Repositories come with community and testing
repositories that are not enabled by default. You could also want to
add the Kiss Linux community repository, or someone's personal kiss
repository.

**Obtaining repositories**

Obtaining a repository is as simple as a git clone. You can do it
with Cem's personal repository as an example, but any other repository
will be the same.

    # Clone the repository to your desired location
    # Optionally add "--depth 1" to save precious disk space
    
    $ git clone --depth 1 git://git.ckyln.com/kiss-repository /var/db/kiss/cem-repository

That's it, you have obtained a new repository!

<strong>Adding a repository to your KISS_PATH</strong>

Now that we have obtained Cem's repository, we can add it to our KISS\_PATH!

We need to edit `/etc/profile.d/kiss_path.sh` for this, but you
can also change your KISS\_PATH from your shellrc.

Add the full path to the repository, in this case
`/var/db/kiss/cem-repository/cem` to the variable
and add a `:` before/after it.

It must be looking like this.

    export KISS_PATH=/var/db/kiss/repo/core:/var/db/kiss/repo/extra:/var/db/kiss/repo/xorg:/var/db/kiss/cem-repository/cem

Update your shell variables by doing `. /etc/profile.d/kiss_path.sh` so you can
immediately access the repository packages


More
----

You should now be able to boot into you Carbs Linux installation. 
To learn more, you can visit the [wiki](//carbslinux.org/wiki)
