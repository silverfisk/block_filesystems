# ec2-instance

Ansible role that formats and mounts block devices according to ```volumes``` variable.

Variables example
---------

```cat vars/myvars.yaml```
 ```yaml
volumes:
  - device_name: /dev/xvdb
    volume_type: gp2
    volume_size: 20
    mount_point: /usr/sap
    fstype: xfs
    encrypt: "yes"
    resizefs: yes
```

Playbook example:
-----------------
```cat playbook.yaml```
```yaml
---

- name: Configure provisioned instances
  hosts: ec2_pets
  become: yes
  gather_facts: true
  pre_tasks:
    - name: "Include global application variables"
      include_vars: "vars/myvars.yaml"
      tags:
        - block_filesystems

  roles:
    - { role: block_filesystems, tags: ["block_filesystems"], mntpoint_mode: 0755 }  # Expects volumes as defined in vars
```