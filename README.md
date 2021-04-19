# Linux Common Commands Library

## Kernel & Processes
```
bg
  Run any suspended job from jobs in the background
cat /etc/issue
  Show current distro
cat /etc/*release
  Show more distro details
fg
  Bring first job to foreground
jobs -l
  List jobs and their pids
kill pid
kill $(pgrep imwheel)
killall ?
pgrep imwheel
pgrep mandb | xargs kill
pkill processname?
ps aux | grep [s]omething
screen somecommand
  Run somecommand, hit ^A to send the proc to the background, screen -r to resume
sleep 1
  Sleep for 1 second
strace somecommand
strace -p pid
  Follow the system calls and signals of a process
timedatectl status
  Show time and status of NTP server
tmux -a
  Attach to the latest running session of tmux; ctrl+b d to detatch
systemctl start systemd-timesyncd
  Start time syncing service (does both system and hardware clocks)
uname -a
  Kernel version (-r for 64-bit or not)
```
## Network
```
ip addr
ip addr flush dev enp0s3
  Remove any acquired IPs. Should be auto-resolved by networkd afterwords, but may need a restart.
journalctl -f
  Follow the journal logs
networkctl
  Very nice alternative to systemctl status systemd-networkd
networkctl status
resolvectl
systemctl restart dhcpcd
  Restart system daemon that acts as our DHCP/DHCPv6 client. Auto-acquires IPs for us on restart.
  https://wiki.archlinux.org/index.php/dhcpcd
systemctl restart systemd-networkd
  Restart system daemon that manages network config.
  https://wiki.archlinux.org/index.php/Systemd-networkd
systemctl status systemd-resolved
  Status of network name resolution systemd service.
```
## File Management
```
dd if=/dev/zero of=/tmp/sample.bin bs=8k count=128k
  Creates a 1GB file and tests write speed
fdisk -l
fdisk -x
  Similar to -l, but with more details (UUIDs)
fdisk /dev/sda
  Load up a disk to check for unused free space on it (m=help, F=free space, q=quit w/o save)
fsck /dev/sda2
  Check/auto-fix busted partitions; should only be run on unmounted drives via a live USB.
  Had to run this twice, once to fix the journal. Again to fix a bunch of other stuff.
hexdump -C filename | less -R
  View binary characters of a file
hdparm -Tt /dev/sda
lsblk
ls -l | awk '{$5=$5*8}1'
  Show bit size of each file, assuming 8 bits/byte
mount /dev/sda1 /newmountpoint
parted /dev/sda unit MB print free
  Displays unused free space for a disk
umount /newmountpoint
xxd filename
xxd -b filename
  Shows all the hex/binary values in a file
```
## User Management
```
chmod +x /somefolder/somefilename
chmod u+x somefile
  Make some file executable; make it executable only for user who owns it
chmod go= somefile
  Remove all group and other permissions
chmod a-rx somefile
  Remove read & execute permissions for all
chown -R usr:grp /somefolderorfile
  Recursively change ownership user+group of some file or folder
chown -R :grp /somefolder/orfile
  Recursively change ownership group of some file or folder
getfacl /home/username
groupadd wsgroup
groups username
history
  Show command line history
less -R /etc/group
less -R /etc/gshadow
less -R /etc/login.defs
less -R /etc/passwd
less -R /etc/shadow
less -R /etc/sudoers
  NEVER manually edit this with vi/vim/nano, use visudo for syntax checking & atomicity
ls /etc/skel/
passwd username
setfacl -m ???
useradd username
userdel username
usermod -aG vboxsf username
  Add user to group vboxsf (may require re-login)
visudo
  
xfce4-session-logout --logout
  Must be excuted as the currently logged in user
xfce4-session-logout --reboot
```
## Desktop Apps
```
dmenu
dmenu_run -l 10
  Awesome, fast, lightweight dynamic menu for X. Keyboard-based popuppable hotbar.
  https://tools.suckless.org/dmenu/
imwheel -b 45
  Fix for any VirtualBox mousehweel issues
mc
  Midnight Commander, tui file manager
nnn
  Amazing tui file manager
  https://wiki.archlinux.org/index.php/Nnn
nemo
  Amazing gui file manager with split screen and tabs
peek
  Record on-screen to a video file
  https://archlinux.org/packages/?name=peek
screenkey
  Awesome application for showing pressed keys
  https://snapcraft.io/install/screenkey/mint
```
## SSH Stuff
```
eval "$(ssh-agent)"
  Sets environment variable for our agent; needed for ssh-add
ssh-add id_rsa
  Add private key for our user to log in with (if not using default ~/.ssh/id_rsa filename)
ssh user@host.ip.address.lol [-i privatekeyfilename]
  Remember to type "yes" to trust that server, or it'll fail: "Host verification auth failed"
ssh-keygen
  Create a public and private key.. afterwards do this:
  cat ~/.ssh/ida_rsa.pub > ~/.ssh/authorized_keys; # Copy id_rsa to your local comp
sudo vi /etc/ssh/sshd_config
  PasswordAuthentication no
scp username@hostname:/path/to/remote/file /path/to/local/file
  https://stackoverflow.com/questions/9427553/how-to-download-a-file-from-server-using-ssh
ssh user@host 'cat /path/on/remote' > /path/on/local
```
## Hardware Details
```
cat /proc/cpuinfo
  CPU details
cat /proc/meminfo
  RAM details
dmesg | grep SATA
dmesg | grep "link up"
  Shows actual capable speeds. 1.5 Gbps=Sata I, 3.0=II, 6.0=III
fdisk -l
  Harddrive info
free
  Free memory on comp
/sbin/hdparm -i /dev/sda2
  Show specific details about a drive (SSD vs etc)
hwclock --show
hwclock --systohc
  Sync hardware clock to match system
lsblk
lspci | grep SATA
  May show actual speeds: "SATA III 6Gb/s"
```
### Dependency Management
```
apt install pkgname
apt update
apt upgrade
pacman -Syu
  Forced full update/upgrade of system (super dangerous)
pacman -Qo /path/to/file
  Find out which package owns a file (such as a man page file)
pacman -Qi pkgname
  Find out why pkg was installed, what depends on it, what it depends on, etc
pacman -S pkgname
  Install a package
pacman -R pkgname
  Remove a package
```
### Shell Options
```
compgen -c
  Show all built-in commands; uses same options as complete built-in command
help compgen
  Simple built-in command help, more details at bottom of man bash
man bash
  Check out all the built-in commands from bash
shopt -s histappend
  Changes history to be append rather than overwrite; preserves between sessions
```
### Video / Screen
```
lspci -v | egrep -i --color 'vga|3d|2d'
  Show current graphics card
xdpyinfo | grep 'dimensions'
  Show current resolution
xrandr | grep '*'
  Show current resolution
xrandr --listmonitors
```