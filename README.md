ansible role for FRP ( Fast Reverse Proxy )
=========

[![Build Status](https://travis-ci.com/jinmiaoluo/ansible-role-frp.svg?branch=master)](https://travis-ci.com/jinmiaoluo/ansible-role-frp)
[![Join the chat at https://gitter.im/ansible-role-frp/Lobby](https://badges.gitter.im/ansible-role-frp/Lobby.svg)](https://gitter.im/ansible-role-frp/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

this role will install frp and setup supervisor for frp to server continuously

Requirements
------------
```
- supervisorctl
- python-pip (base on python 2.x, why? please refer this issue https://github.com/geerlingguy/ansible-role-supervisor/issues/15)
```

Role Variables
--------------

```
---
# defaults file for frp
frp_version: 0.21.0
arch: linux
bit: amd64
install_frpc: yes
frpc_config_path: "frpc.ini"
install_frps: no
frps_config_path: "frps.ini"
```

Dependencies
------------

```
- geerlingguy.pip
- geerlingguy.supervisor
```

config for geerlingguy.supervisor(please consider to change password for supervisor by set supervisor_password)
```
supervisor_user: root
supervisor_password: root
supervisor_programs:
  - name: 'frpc'
    command: '/usr/local/bin/frpc -c /etc/frpc.ini'
    state: present
    configuration: |
      numprocs=1
      user=root
      killasgroup=true
      stopasgroup=true
      autostart=true
      autorestart=true
      startretries=10000000
      startsecs=5
      redirect_stderr=true
      stdout_logfile=/var/log/supervisor/%(program_name)s.log
      stdout_logfile_maxbytes=100MB
      stdout_logfile_backups=10

# # if your wanna install frps, please uncomment this section
#  - name: 'frps'
#    command: '/usr/local/bin/frps -c /etc/frps.ini'
#    state: present
#    configuration: |
#      numprocs=1
#      user=root
#      killasgroup=true
#      stopasgroup=true
#      autostart=true
#      autorestart=true
#      startretries=10000000
#      startsecs=5
#      redirect_stderr=true
#      stdout_logfile=/var/log/supervisor/%(program_name)s.log
#      stdout_logfile_maxbytes=100MB
#      stdout_logfile_backups=10
```

config for geerlingguy.pip
```
pip_package: python-pip
pip_install_packages:
  - name: supervisor
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
- hosts: servers
  become: yes
  roles:
    - geerlingguy.pip
    - geerlingguy.supervisor
    - jinmiaoluo.frp
```

License
-------

BSD
