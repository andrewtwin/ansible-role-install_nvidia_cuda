Role Name
=========

Install pre-compiled or dkms Nvidia drivers and cuda-toolkit on RHEL8 or RHEL9

Automates the instructions from
- https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/
- https://docs.nvidia.com/cuda/cuda-installation-guide-linux/

Requirements
------------

See Nvidia docs for system requirements.

Role Variables
--------------

`driver_version` - Default "open-dkms". One of the precompiled-streams from  https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/index.html#precompiled-streams 

Dependencies
------------

- `community.general.dnf_config_manager` module

Example Playbook
----------------

Can test the ouput of `lspci` to only run on nodes with Nvidia GPU hardware
```
---
- name: Install CUDA on nodes with GPUs
  hosts: all
  become: True
  gather_facts: True
  pre_tasks:
  - name: Check which nodes have NVIDIA GPUs
    register: lspci_result
    changed_when: False
    ansible.builtin.command:
      cmd: "lspci"

  tasks:
  - name: Install CUDA
    when: lspci_result['stdout'] | regex_search('vga.*nvidia', ignorecase=True, multiline=False)
    ansible.builtin.include_role:
      name: install-nvidia-cuda
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
