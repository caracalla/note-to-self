Installing Debian

# within the installer
install ssh-server and standard utilities

# in virtualbox settings
set up second network adapter - vboxnet0

# install shit
as root:
* `apt-get install sudo vim git build-essential curl tmux`
* `update-alternatives --config editor`
* `vim /etc/sudoers`

# set up networking
in the host machine:
* get the ip in syspref > network (not necessary?)

in virtualbox:
* in machine settings, set adapter 2 to vboxnet0 host only adapter

in VM:
* `vim /etc/network/interfaces`, add the following (for jessie)
    ```
    # host only
    auto eth1
    iface eth1 inet static
    address [server address from above plus a few numbers (192.168.57.1 -> 192.168.56.10)]
    netmask 255.255.255.0
    ```
* `/etc/init.d/networking restart`
* `ping 8.8.8.8`

# set up SSH
in VM:
* `vim /etc/ssh/sshd_config`
    ```
    PermitRootLogin yes
    ```
* `service sshd restart`

in host machine:
* copy value of `~/.ssh/id_rsa.pub`
* `ssh root@[host ip]`

in VM:
* paste value of `id_rsa.pub` into `~/.ssh/authorized_keys` file
* make a new keypair and add it to github

# clone dotfiles
in VM:
* clone dotfiles
* `./remove_symlinks.sh`
* `./setup.sh`
* `cp linux/.tmux.conf ~`



accord: 1990-1993
civic: 1987-1991
