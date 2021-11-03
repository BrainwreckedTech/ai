# AI (Assisted Installer)
AI is a set of scripts utilizing (X)dialog tp help the user set up a
new system.  The process is divided into five independent parts,
so you can omit the use of any assistance you don't want.

## Philosophy

* The user should be in control of most all of the process.
* Only stop the user from making invalid decisions
    -  Like setting an EFI System Partition size less than the minimum
	   for the intended filesystem (32MiB for FAT32).
* Suggestions and defaults can be provided, but the user should retain
  ultimate control.
* When intervening on the user's behalf, only changes that make no
  difference either way should go unprompted
    - like when adding a systemd override for sshd to add
	  `After=network-online.target` to the `Unit` section of the service
	  file.
* Otherwise, the user should be prompted if they want something done.

## `ai-parts`
Partition a disk for use in either a single-boot Linux-only or 
dual-boot Linux/Windows setup.  Takes full consideration of both
BIOS/UEFI and MBR/GPT (and the fact that Windows turns its nose
up at UEFI+MBR).

STATUS: Works, stable.

TODO:  There's no consideration made to having existing partitions from
an OEM Windows setup.  In this scenario, once the user has shrunk the
Windows System Partition to something minimal, `ai-parts` could make
new partitions in the available space while leaving existing partitions
alone.

TODO:  Allow for a config file in the user's home directory that the
user can set their preferred defaults.

## `ai-mkfs`
Formats partitions and creates a "helper" `fstab` in your home
directory for use later.  For BTRFS, subvolumes can be set up ahead of
time.  If the appropriate utilities are installed, will turn on
compression for F2FS volumes.

STATUS: Works, stable.

## `ai-pkgs`
Installs packages into the new Linux environment.  Extra packages can
be made available if you have unofficial 3rd-party repos active like
chaotic-aur.  (Anything not in the official repos should be checked
with IS_REPO_PKG first.)

STATUS: No chance to go back and correct mistakes.
Only supports Arch Linux.  Otherwise works and is stable.

TODO:  Users can add packages of their choice using the `packages`
file, but that option only shows up before the user sets up a GUI
environments.  Users should be able to do the same for GUI programs.

TODO: Support is planned for other Linux distributions that can use the
`chroot` method of installation.  Currently only Arch Linux is supported.

## `ai-confs`
Configures the new system.
Does as much work as possible without chrooting in.

STATUS: Nearly complete -- all base functionality present except running mkinitcpio.

The overview menu will show the configuration status of these, but
there's no function yet to actually change the configuration.

## `ai-boot`
Configures the bootloader of the new system

STATUS: Work in progress.

| Firmware | Bootloader   | Installation | Tools | Hook | Stanza | Configure |
|:--------:|:------------:|:------------:|:-----:|:----:|:------:|:---------:|
| BIOS     | GRUB 2       |✔|✅|?|❌|❌|
| UEFI     | GRUB 2       |✔|✅|?|❌|❌|
| UEFI     | rEFInd       |✔|✔|✔|✔|✔|
| BIOS     | SysLinux     |✔|✔|?|✔|✔|
| UEFI     | SysLinux     |✔|ⁿ∕ₐ|?|✔|✔|
| UEFI     | systemd-boot |✔|✔|✔|✔|✔|

