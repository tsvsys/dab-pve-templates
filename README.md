```
templates for Proxmox VE 7
built by Debian Appliance Builder (dab)
source https://github.com/tsvsys/dab-pve-templates

changelog:

set the PermitRootLogin to yes in /etc/ssh/sshd_config
removed postfix from all images.
pre-generated locale en_US.UTF-8
timezone set to Europe/Sofia

installed additional packages: 
bzip2, wget, dialog, traceroute, logrotate, net-tools, vim-nox

added a script /etc/rc.local and enabled the rc-local.service systemd unit 
in order to show the VM IP address in a ProxmoxVE VM Console
```
