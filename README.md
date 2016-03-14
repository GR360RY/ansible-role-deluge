
deluge
======
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.deluge-green.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/deluge/)

An Ansible role to setup and configure Deluge Daemon and Deluge-Web on Ubuntu.


Requirements
------------

This role requires Ansible 2.0 or higher. Platform requirements are listed in the metadata file.
Make sure to download roles specified in **Dependencies** section if role installed **not** with Ansible Galaxy.

Overview
--------

List of tasks that will be performed under `deluge` role:

1. Install and Configure Deluge Daemon
2. Install and Configure Deluge Web Daemon
3. Create Deluge Complete and Incomplete Downloads folders
4. Configure Label and Exectute plugins if installed with `nzbtomedia` roles

This role is dependant on `htpc-common` role which performs the following tasks:

1. Install ssh server to allow remote management.
2. Configure Zerconf networking and avahi-alias service.
3. Create htpc_user if user doesn't exist.
4. Enable sudo access for htpc user.
5. Create htpc media and downloads folders.

Downloads and Media folders layout if used with default variable values:

```
/mnt/media/
├── downloads               
│   ├── complete        # Complete Downloads
│   └── incomplete
│       ├── deluged     # Deluge Incomplete Downloads
│       └── process     # nzbtomedia processing folders
│           ├── movie
│           └── tv
├── movies
├── music
├── pictures
└── tv
```

* Default password for Deluge Web Interface is set to `deluge`
* If used with default port values, access to web interface is available at http://localhost:8112/

Role Variables
--------------

```
# defaults file for deluge

# Helper vaiable. In use by other roles
deluge_enabled: yes

# Deluge Daemon Path
deluged_path: /opt/deluged

# Deluge Incomplete download locations
deluged_incomplete: "{{ htpc_downloads_incomplete }}/deluged"

# Password for localclient user.
deluged_localclient_password: 2b9cf85259f2149da47458eda73ba23ac06faa21

# Helper vaiable. In use by other roles
deluged_host: "{{ ansible_default_ipv4.address }}"

deluged_port: 58846
deluge_web_port: 8112
```

Dependencies
------------

Role Name | Description
----------|-----------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common/)| Create htpc user and media folders
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.nzbtomedia-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/nzbtomedia)    | Install NZBtoMedia Postprocessing

Variables defined in `GR360RY.htpc-common` role:

```
# defaults file for htpc-common

htpc_user_username: htpc
htpc_user_password: htpc
htpc_user_group: htpc
htpc_user_shell: /bin/bash
htpc_user_sudo_access: yes
htpc_ssh_service: yes
htpc_create_media_folders: yes
htpc_zeroconf: yes
htpc_media_path: /mnt/media
htpc_media_movies: movies
htpc_media_tv: tv
htpc_media_music: music
htpc_media_pictures: pictures
htpc_downloads_complete: "{{ htpc_media_path }}/downloads/complete"
htpc_downloads_incomplete: "{{ htpc_media_path }}/downloads/incomplete"
```

Variables defined in `GR360RY.nzbtomedia` role:

```
---
# defaults file for nzbtomedia

nzbtomedia_enabled: yes
nzbtomedia_path: /opt/nzbtomedia
```

Example Playbook
----------------

Install deluge for user `foo`. Don't configure ssh,sudo and bonjour/zeroconf. Skip media folders creation.

```
---
- hosts: htpc-server
  become: yes

  vars:

  	htpc_user_username: foo
  	htpc_user_group: foo

    htpc_user_sudo_access: no
  	htpc_user_ssh_service: no
  	htpc_create_media_folders: no
  	htpc_zeroconf: no

  	htpc_downloads_complete: /home/foo/Downloads
  	deluged_incomplete: /home/foo/.deluged_incomplete

  roles:
    - role: GR360RY.deluge
```

Install Couchpotato and Sickrage with Deluge Torrent client as Downloader.
Create user `htpc` identified by password `htpc`. Use default folders layout.

```
- hosts: htpc-server
  become: yes

  roles:
    - role: GR360RY.deluge
    - role: GR360RY.sickrage
    - role: GR360RY.couchpotato
     
```

HTPC-Ansible Project
--------------------

This role is part of HTPC-Ansible project that includes additional roles for building Ubuntu Based HTPC Server.

 Role name               | Comment
-------------------------|-----------------------------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common)   | Create htpc user and media folders
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--nas-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-nas)         | Configure NAS ( NFS, CIFS and AFP )
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--client-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/kodi-client)   | Install Kodi Media Player
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--mysql-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/kodi-mysql)     | Install MySQL Backend for Kodi
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.deluge-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/deluge)              | Install Deluge Bittornet Client
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.nzbtomedia-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/nzbtomedia)      | Install NZBtoMedia Postprocessing
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sickrage-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/sickrage)          | Install SickRage
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.couchpotato-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/couchpotato)    | Install CouchPotato
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--manager-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-manager) | Install htpc-manager
<!--
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sabnzbd-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/sabnzbd)            | Install Sabnzbd
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.tvheadend-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/tvheadend)        | Install Tvheadend

Additional Info is available at [www.htpc-ansible.org](http://www.htpc-ansible.org)
 -->
License
-------

BSD

Author Information
------------------

Gregory Shulov
