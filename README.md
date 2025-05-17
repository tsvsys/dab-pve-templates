```
templates for Proxmox VE 7.x/8.x
built by Debian Appliance Builder (dab)
source https://github.com/tsvsys/dab-pve-templates

changelog:

set the 'PermitRootLogin yes' in /etc/ssh/sshd_config
set the 'UseDNS no' in /etc/ssh/sshd_config
removed postfix from all images.
pre-generated locale en_US.UTF-8
timezone set to Etc/UTC (can be changed in Makefile)
set APT::Install-Recommends "false"; to /etc/apt/apt.conf.d/00InstallRecommends

installed additional packages: 
bzip2, wget, dialog, traceroute, logrotate, net-tools, vim-nox, python3

added a script /etc/rc.local and enabled the rc-local.service systemd unit 
in order to show the VM IP address in a ProxmoxVE VM Console

NOTE: ALL templates are set to zstd format, they will work only on PVE v7/v8
NOTE: in order to build debian 13 version template - you need to have dab 3.7.0 installed on your build host
```
