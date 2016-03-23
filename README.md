BackupPC Ansible role
=====================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-HanXHX.backuppc-blue.svg)](https://galaxy.ansible.com/HanXHX/backuppc) [![Build Status](https://travis-ci.org/HanXHX/ansible-backuppc.svg?branch=master)](https://travis-ci.org/HanXHX/ansible-nginx) 

This role installs and configures Backuppc. It works on Debian Jessie. It can work on Ubuntu or other Debian-based systems. But no direct support will be added (PR accepted).

Requirements
------------

None.

Note
----

At this time, this role only manage backup via rsync (+ ssh) method.

Role Variables
--------------

### Role var

- `backuppc_server_name`: fqdn of the backup server
- `backuppc_ssh_key_bits`: set length of ssh key pair (optional, default 2048 by Ansible)
- `backuppc_fetch_ssh_key`: copy backkupc ssh key from server (boolean)
- `backuppc_local_fetch_dir`: local dir where you fetch backuppc SSH public key
- `backuppc_hosts`: clients list to backup (see below)

### Client vars

Each client configuration override global configuration.

- `hostname`: (M) hostname of the host
- `state`: (O) absent or present (default)
- `include_files:`: (O) default files (directories) list to backup.
- `exclude_files:`: (O) default files (directories) list to exclude in backup
- `more`: (O) hash with specific key/value (usefull for custom directives)

(O): Optional (M): Mandatory


### Global configuration

You should [RTFM](http://backuppc.sourceforge.net/faq/BackupPC.html) for these variables:

- `backuppc_ServerPort`
- `backuppc_ServerMesgSecret`
- `backuppc_MaxBackups`
- `backuppc_MaxUserBackups`
- `backuppc_MaxBackupPCNightlyJobs`
- `backuppc_BackupPCNightlyPeriod`
- `backuppc_MaxOldLogFiles`
- `backuppc_FullPeriod`
- `backuppc_IncrPeriod`
- `backuppc_PingMaxMsec`
- `backuppc_RsyncClientCmd`
- `backuppc_RsyncClientRestoreCmd`
- `backuppc_BackupFilesOnly`
- `backuppc_BackupFilesExclude`
- `backuppc_XferLogLevel`

Other global configuration can be managed (you can create issues or PR).

Notes
-----

### About HTTP

This role doesn't manage any webserver! You _must_ use another role to install a HTTP service _before_ this role. It doesn't manage any authentication.

Be careful, in Debian based systems, backuppc depends [httpd virtual package](https://packages.debian.org/jessie/httpd). If you don't install any HTTP server, Debian will install Apache2. However, you can use [my Nginx role](https://github.com/HanXHX/ansible-nginx) which is compatible with this role.

### About backups

This role downloads backuppc SSH public key. You must deploy it on each clients!

Dependencies
------------

None.

Example Playbook
----------------

### Quick and dirty

You should look at [defaults/main.yml](defaults/main.yml).

### With my Nginx role

```
- hosts: backup
  vars:
    nginx_vhosts:
      - name: backup.mydomain.tld
        template: _backuppc
		htpasswd: backuppc
    nginx_htpasswd:
      - name: backuppc
        description: 'Please login'
        users:
          - name: hx
            password: myPassword
  roles:
    - HanXHX.nginx
    - HanXHX.backuppc
```

License
-------

GPLv2

Author Information
------------------

- Twitter: [@hanx_hx](https://twitter.com/hanxhx_)
