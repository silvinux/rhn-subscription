Role Name
=========
Role to register a server to the RHN, update system and remove/install packages.

Requirements
------------
Must have an active RH subscription!

Role Variables
--------------


Dependencies
------------
NA

Example Playbook
----------------
USAGE:   

# ansible-vault create vars/vault.yml
# ansible-vault encrypt vars/vault.yml
# ansible-vault edit vars/vault.yml
# ansible-playbook -i inventory test.yml -k --ask-vault-pass
vars/vaul.yml

---
vault_rhn_user: redhat-support-active-account 
vault_rhn_pwd: p4$$W0rD
vault_rhn_pool_id: 01123581321245589144233377610987

playbook.yml
---
- hosts: lab
  gather_facts: yes
  remote_user: silvinux
  become: yes
  become_method: sudo
  roles:
    - role: ../../install-idm-server
      register_rhn: true
      rhn_user: "{{ vault_rhn_user }}"
      rhn_pwd: "{{ vault_rhn_pwd }}"
      rhn_pool_name: "{{ vault_rhn_pool_id }}"
      OCP_VERSION: 3.11
      ANSIBLE_VERSION: 2.7
      rhel_repos:
        - rhel-7-server-rpms
        - rhel-7-server-extras-rpms
        - rhel-7-fast-datapath-rpms
        - rhel-server-rhscl-7-rpms
        - rhel-7-server-optional-rpms
        - rhel-7-server-supplementary-rpms
        - "rhel-7-server-ose-{{ OCP_VERSION }}-rpms"
        - "rhel-7-server-ansible-{{ ANSIBLE_VERSION }}-rpms"
      packages_uninstall:
        - cloud-init
      packages_install:
        - vim
        - vim-enhanced
        - nmap
        - net-tools
        - ansible
        - git
        - bind-utils
      hostname_full: idm.lab.example.com
      hostname_short: idm
      external_ip: 192.168.122.13
      realm: LAB.EXAMPLE.COM
      domain: lab.example.com
      dm_pass: redhat1234
      admin_pass: Redhat1234
      forward_ip: 8.8.8.8

License
-------

GPLv3

Author Information
------------------
silvinux - silvinux7@gmail.com
