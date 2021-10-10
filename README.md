# AI (Assisted Installer)
AI is a set of scripts utilizing (X)dialog tp help the user set up a
new system.

The process is divided into four parts.  Each part is independent of
the other, so you can omit the use of any assistance you don't want.

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

STATUS: Works, stable, only one bug left to consider.

TODO: `ai-parts` uses some invalid/unconventional partition types to
flag intentions to `ai-mkfs` for a couple of partitions that don't have
a dedicated type.  `ai-mkfs` should set these partition types back to
something valid/conventional.

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

