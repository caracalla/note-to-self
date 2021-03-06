# Installing Debian on a VM with VirtualBox

1. Download a Debian image - this guide is based on Jessie
1. Download VirtualBox
1. Do all the stuff to make a Linux VM within VirtualBox

## within the Debian installer
install ssh-server and standard utilities

## install shit
as root:
* `apt-get install sudo vim git build-essential curl tmux`
    * If it asks for a cdrom, `vi /etc/apt/sources.list` and comment out cdrom lines at the top
* `update-alternatives --config editor`
* `vim /etc/sudoers`
    * dear past self: what was I supposed to do here?

## set up networking
in the host machine:
* get the ip in syspref > network (not necessary?)

in virtualbox:
1. File > Host Network Manager
    1. If a host-only network (vboxnet0) doesn't already exist, click "Create"
1. VM Settings > Network > Adapter 2
    1. Enable Network Adapter
    1. Attached to: Host-only Adapter
    1. Name: vboxnet0

in VM:
* `vim /etc/network/interfaces`, add the following (for jessie)
    ```
    # host only
    auto eth1
    iface eth1 inet static
    address [server address from above plus a few numbers (192.168.57.1 -> 192.168.56.10)]
    netmask 255.255.255.0
    ```
    * for stretch, run `ls /sys/class/net` and replace `eth1` with the second one
* `/etc/init.d/networking restart`
* `ping 8.8.8.8`

## set up SSH
in VM:
* `vim /etc/ssh/sshd_config`
    ```
    PermitRootLogin yes
    ```
* `service sshd restart`
* `ssh root@192.168.56.10`
    * should prompt for root password
* copy value of `~/.ssh/id_rsa.pub` from host machine
* paste value of `id_rsa.pub` into `~/.ssh/authorized_keys` file
* `ssh root@192.168.56.10`
    * should use ssh key

## clone dotfiles
in VM:
* create a new keypair and add it to github
    * `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"``
    * https://github.com/settings/ssh/new
* clone dotfiles
* `./remove_symlinks.sh`
* `./setup.sh`
* `cp linux/.tmux.conf ~`
