# chroot-rsync

quick script for setup rsync with chroot.

Tested only on Ubuntu 20/22.

## Setup chroot files

This will create hard links to required files (same filesytem)

```
locate_deps /bin/sh	/storage/submit/<user>
locate_deps /usr/bin/rsync /storage/submit/<user>
cp chroot-rsync /storage/submit/<user>usr/bin/
```

## SSH configuration

```
Match Group rsync-only
  AuthorizedKeysFile /etc/ssh-pool/%u
  ChrootDirectory /storage/submit/%u
  ForceCommand /usr/bin/chroot-rsync
  AllowTcpForwarding no
  X11Forwarding no
  PermitTTY no
```

## Create user & group

```
useradd -M -d /submit -s /bin/sh <user>
groupadd rsync-only
gpasswd -a <user> rsync-only
mkdir -p /storage/submit/<user>/proc
mkdir -p /storage/submit/<user>/bk
```

## Mount procfs

For example via /etc/fstab
```
proc  /storage/submit/<user>/proc  proc  hidepid=2 0 0
```

## Mount (or bind) storage folder

``
/backup/<user>	/storage/submit/<user>/bk	none	bind	0	0
``
