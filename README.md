ansible-keepalived: Ansible role for setting up keepalived 
============================================================

This role has been expanded greatly compared to the original.

TODO: Fix examples and explain how they work.

This role sets up keepalived and has a few more advanced options than pervius iterations.
It supports multiple VIPs and interfaces now, with multiple instances.

Its worth configuring all the variables as per below, to get the best config.
In particular use_vmac is now the default, so this requires that kernel support, which
should be most current distros.

Keepalived can watch processes to influence on which node should be the master. Setting
variable "keepalived_check_process" to the name of the process will do. I use keepalived
to give high availability to haproxy, so I use.

keepalived_check_process: "haproxy"

Dependencies
------------
Works with centos/redhat and Debian/Ubuntu

Role Variables
--------------

Example playbook with all the variables filled out:
```
  vars:
    keepalived_instance:
      - name: "vrrpnic"
        primary_interface: "enp4s0"
        router_id: "69"
        auth_pass: "fwvips"
        track_interface: "bond0"
        instance_vips:
          - address: "192.168.253.253"
            interface: "bond0"
          - address: "24.24.24.24"
            interface: "enp1s0"
      - name: "sharednic"
        primary_interface: "bond0"
        router_id: "69"
        auth_pass: "fwvips"
        track_interface: "enp4s0"
        instance_vips:
          - address: "192.168.253.253"
            interface: "bond0"
          - address: "24.24.24.24"
            interface: "enp1s0"

    keepalived_sync_group: true
    keepalived_priority: 59
    keepalived_backup_priority: 123
```

Note that keepalive_sync_group creates a group of instances work on different interfaces.
This means you can have 1 interface for dedicated traffic and another for shared traffic, 
providing some redundancy to the VRRP traffic.
In the above example bond0 is the main interface, enp4s0 is the dedicated vrrp interface and enp1s0 is another
internet facing interface. Note also that keepalived_role: MASTER or BACKUP is set in the host_vars of the target machines.




License
-------
Apache

Author Information
------------------
Dave Kempe and various at Sol1

with thanks to original author:

Toni Comerma - tcomerma (at) gmail.com

Collaborator Information
------------------------

Pablo Estigarribia - pablodav (at) gmail dot com
