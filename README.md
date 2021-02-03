paulrentschler.tsmbackup
========================

[![MIT licensed][mit-badge]][mit-link]

Installs and configures the IBM Spectrum Protect (formally Tivoli Storage Manager) backup client on Ubuntu Linux


Requirements
------------

An IBM Spectrum Protect (TSM) server must be available to connect to. The server's fully qualified domain name (FQDN), communication method (typically TCP/IP), and port will be needed.


Role Variables
--------------

The following variables are available with defaults defined in `defaults/main.yml`:

The url to download the client tarball from IBM. This value is further defined based on the operating system of the host and only needs to be specified if you want a different version.

    tsmbackup_client_url: ""


Specify the directories to be backed up.

    tsmbackup_backup_dirs: []


Use the following to backup all files on the host.

```yaml
tsmbackup_backup_dirs:
  - all-local
```


Specify the files/directories to exclude.

```yaml
tsmbackup_exclusion_files:
  - "/etc/shadow"
tsmbackup_exclusion_dirs:
  - "/proc"
```


The backup is scheduled to run using a cron task. These settings allow you to customize when that backup runs. The default is every day at 6am.

    tsmbackup_run_hour: 6
    tsmbackup_run_min: 0
    tsmbackup_run_month: "*"
    tsmbackup_run_day: "*"
    tsmbackup_run_days: "*"


Fully qualified domain name (FQDN) of the client (host where this is being installed) to identify it to the server.

    tsmbackup_nodename: "{{ inventory_hostname }}"


Username of the user accessing the server on behalf of the client.

    tsmbackup_user: "{{ devops_user|default(ansible_user) }}"


Specify the TSM backup server and how to connect to it.

    tsmbackup_server: ""
    tsmbackup_server_comm_method: TCPip
    tsmbackup_server_port: 1500


Specify whether to continue compressing files even if their size increases through the use of compression. (TSM defaults to Yes)

    tsmbackup_compress_always: "No"


Specify if symlinks should be followed and the target is backed up (Yes) or if just the symbolic link is backed up (No). Valid with the "archive" command ONLY!

    tsmbackup_archive_symlink_as_file: "No"


Specify if symlinks should be followed during incremental backups and if files should be restored to the target of the symlink. (TSM defaults to No) Valid with the "incremental", "restore", and "retrieve" commands.

    tsmbackup_follow_symlinks: "Yes"


The log files for the scheduler and errors.

    tsmbackup_scheduler_log: "/var/log/dsmsched.log"
    tsmbackup_error_log: "/var/log/dsmerror.log"


Dependencies
------------

None.


Example Playbook
----------------

Basic example that specifies the minimal values required.

```yaml
---
- hosts: all
  roles:
    - role: paulrentschler.tsmbackup
      vars:
        tsmbackup_server: backups.example.com
        tsmbackup_backup_dirs:
          - /home
          - /var/logs
```


License
-------

[MIT][mit-link]


Author Information
------------------

Created by Paul Rentschler in 2021.

[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge
[mit-link]: https://github.com/paulrentschler/ansible-role-tsmbackup/blob/master/LICENSE
