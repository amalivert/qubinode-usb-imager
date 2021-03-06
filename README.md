qubinode-usb-imager
=========

The qubinode-usb-imager Ansible role builds a bootable usb disk to be used to a qubinode hosts

Requirements
------------

- RHEL Server ISO


Download ISO
------------
* [Red Hat Enterprise Linux 7.6 Binary DVD](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.6/x86_64/product-software)
* [Red Hat Enterprise Linux 7.7 Binary DVD](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.7/x86_64/product-software)
* [Red Hat Enterprise Linux 7.8 Beta Binary DVD](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.8%20Beta/x86_64/product-software)

Creating SHA-512 password for qubinode_user_pw and root_pw
------------
```
mkpasswd --method=SHA-512
```

Role Variables
--------------

| Parameter | Default value | Description |
| --- | --- | --- |
| iso_grub_dir  | default = [ '/rhel-server-7.7-x86_64-dvd.iso' ]  | set ISO variable in qubinode kickstart file  |
| rhel_qcow_dir | default = '/home/qubi/rhel-server-7.7-update-2-x86_64-kvm.qcow2' | default RHEL qcow images to be used in qubinode installation | 
| rhel_qcow_file | default = /rhel-server-7.7-update-2-x86_64-kvm.qcow2 | default RHEL qcow images to be used in qubinode installation | 
| qubinode_github | default = https://github.com/Qubinode/qubinode-installer/archive | qubinode url to pull qubinode code | 
| qubinode_branchname | default = 2.2 | qubinode releasae branch that you would like to use | 
| rhel_version  | default = 7.7 | set RHEL Version  |
| ks_file | default = 'qubinode-kickstart.ks', options: qubinode-kickstart.ks, x11sdv-8c-tp8f.ks, qubinode_rhel.ks | set KS variable in qubinode kickstart file |
| ks_file_dir   | default = '/{{ ks_file }}'   |  set KS variable directory  |
| packages | default = [ 'grub2-efi', 'shim', 'gdisk', 'grub2-efi-modules', 'grub2-efi-x64-modules' ] | required packages |
| qubinode_hostname | default = 'qubinode-box.example.com' | hostname for qubinode server |
| set_static_ip  | true  | Configures machine with static ip  |
| qubinode_ip_addr | | qubinode host default network ip address |
| qubinode_gw | | qubinode host default network gateway
| qubinode_nameserver_ip | default = '1.1.1.1' | DNS server for qubinode server |
| qubinode_net_dev | | qubinode network device(exanple: 'eno1')
| qubinode_netmask | | qubinode host default network netmask(example: '255.255.255.0) |
| rhel_iso_dir | | location  of rhel-server-7.7-x86_64-dvd.iso (example: '/home/qubiuser/rhel-server-7.7-x86_64-dvd.iso') |
| root_pw | | root password for qubinode box
| usb_device | | example: '/dev/sdc' |
| enable_gnome_desktop  | false  |  Set to true if you would like to install gnome desktop.  |
| qubinode_user_pw | | qubinode host default qubi user password |
| qubinode_username | default = 'qubi'| qubinode admin user username |
| qubinode_user_fullname | default = 'Qubi Admin'| qubinode admin user full name |
| ok_to_reboot | default = 'no' | reboot your workstation/host if partprobe fails |
| os_disk | default = 'sda' | the name of the first disk on your device where the os gets installed |

Example Playbook for Generic Server
----------------
```
---
- hosts: localhost
  remote_user: root
  roles:
    - name: qubinode-usb-imager
      vars:
        rhel_image_dir: '/home/qubi/Downloads'
        usb_device: '/dev/sdb'
        root_pw: "$6$lzcUgJ886.GHT1IM$BtYRQltzadzbHtubxHC1li5yFbdvdkTeGnD2ex1H4VHwQoUGTz22UHyUondkHu/wG515sFuztuesrwC7s.Xkd/"
        set_static_ip: true
        qubinode_user_pw: "$6$hDS1K0FLywm2VIHm$c3PP8Ko9eHxYS.Lk/gRtwYzQCBlm0otDpx7UlJDuTYeK0EtUG40kS/gXKgMAaZ71NavoEsCHTnamQVCuofQh1/"
        qubinode_username: 'qubi'
        qubinode_username_fullname: 'Qubi Admin'
        qubinode_net_dev: 'eno1'
        qubinode_ip_addr: '192.168.86.249'
        qubinode_nameserver_ip: '1.1.1.1'
        qubinode_netmask: '255.255.255.0'
        qubinode_hostname: 'qubinode-box.example.com'
        qubinode_gw: '192.168.86.1'
        qubinode_user: 'qubi'
        iso_file: 'rhel-8.2-x86_64-dvd.iso'
        qcow_image_file: 'rhel-8.2-x86_64-kvm.qcow2'
        rhel_os_major_version: '8'
        rhel_os_minor_version: '2'
        qubinode_github: 'https://github.com/Qubinode/qubinode-installer/archive/'
        git_branch_name: 'dev'
        ks_file: 'qubinode_rhel.ks'
        ok_to_reboot: no
```

Playbook for Super Micro Server with X11SDV-8C-TP8F motherboard
----------------
```
- hosts: localhost
  remote_user: root
  roles:
    - name: qubinode-usb-imager
      vars:
        rhel_iso_dir: '/home/qubi/rhel-server-7.7-x86_64-dvd.iso'
        rhel_qcow_dir: '/home/qubi/rhel-server-7.7-update-2-x86_64-kvm.qcow2'
        rhel_qcow_file: '/rhel-server-7.7-update-2-x86_64-kvm.qcow2'
        qubinode_github: 'https://github.com/Qubinode/qubinode-installer/archive/'
        qubinode_branchname: '2.2'
        rhel_version: 7.7
        usb_device: '/dev/sdb'
        os_disk: sda
        root_pw: "$6$lzcUgJ886.GHT1IM$BtYRQltzadzbHtubxHC1li5yFbdvdkTeGnD2ex1H4VHwQoUGTz22UHyUondkHu/wG515sFuztuesrwC7s.Xkd/"
        qubinode_user_pw: "$6$hDS1K0FLywm2VIHm$c3PP8Ko9eHxYS.Lk/gRtwYzQCBlm0otDpx7UlJDuTYeK0EtUG40kS/gXKgMAaZ71NavoEsCHTnamQVCuofQh1/"
        set_static_ip: false
        qubinode_username: 'qubi'
        qubinode_net_dev: 'eno1'
        qubinode_ip_addr: ''
        qubinode_nameserver_ip: ''
        qubinode_netmask: ''
        qubinode_hostname: 'qubinode-box.example.com'
        qubinode_gw: ''
        iso_grub_dir: '/rhel-server-7.7-x86_64-dvd.iso'
        enable_gnome_desktop: false
        ks_file: 'x11sdv-8c-tp8f.ks'
        ok_to_reboot: no
```

Known Issues
------------

If you try to mount the data volume and encounter an error like this.

```
mount: wrong fs type, bad option, bad superblock on /dev/xxx
```

Using the device sdb2 as an example, run the following commands to resolve.

```
xfs_repair -L /dev/sdb2
xfs_admin -U generate /dev/sdb2
```

License
-------

BSD

Author Information
------------------
Abner Malivert
