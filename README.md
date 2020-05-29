# Ansible BigBlueButton Installation Role

Ansible role to install [_BigBlueButton_](https://www.bigbluebutton.org) web conferencing platform with SSL support out of the box using [_LetsEncrypt_](https://letsencrypt.org/).

The role follows _BigBlueButton_ official [installation instructions](http://docs.bigbluebutton.org/install/install.html).

Forked from [softwaremill/ansible-bigbluebutton](https://github.com/softwaremill/ansible-bigbluebutton), as it seems abandoned. Itr has the following additional features:

## Features
  * Installs latest stable version (currently _2.2_).
  * Installation behind a firewall (NAT setup support)
  * Automatic SSL configuration using _LetsEncrypt_ certificates using [thefinn93/ansible-letsencrypt](https://github.com/thefinn93/ansible-letsencrypt) role.
  * Optionally installs the demo and check packages.

## Supported Platforms
As this role follows the official installation instructions, the supported OS version is the one specified there: Ubuntu (16.04 for the current bbb version).

Requires Ansible >= 2.4.0

## Usage

To get up _BigBlueButton_ up and running the following variables can be configured:

  * `bbb_server_names`:  Set the list of FQDN hostnames that point to the server where _BigBlueButton_ is going to be installed. If only a single FQDN is required, `bbb_server_name` can be used instead. Setting either one of these is the only mandatory value, the playbook will fail if neither variable is defined.
  * `bbb_configure_firewall`: Configure local UFW firewall on server (Default: `True`).
  * `bbb_configure_nat`: Configure NAT support for servers behind an external firewall (Default: `False`).
  * `bbb_configure_ssl`: Configure SSL support using _LetsEncrypt_ certificates (Default: `False`).
  * `bbb_ssl_email`: Set _LetsEncrypt_ authorization email address.
  * `bbb_install_html5`: Install the HTML5 client (Default: `True`).
  * `bbb_install_demo`: Install the bbb-demo package, useful to easily test the new installation is working (Default: `False`).
  * `bbb_install_check`: Install the bbb-check package, useful to debug the new installation if having issues (Default: `False`).
  * `bbb_install_playback_notes`: Install the bbb-playback-notes package to play back shared notes from a recorded session (Default: `True`).
  * `bbb_install_playback_podcast`: Install the bbb-playback-podcast package to play back audio from a recorded session (Default: `True`).
  * `bbb_install_playback_screenshare`: Install the bbb-playback-screenshare package to play back shared screens from a recorded session (Default: `True`).
  * `bbb_install_webhooks`: Install the bbb-webhooks package, useful to integrate bbb into other web applications (Default: `True`).
  * `bbb_install_greenlight`: Install the Greenlight frontend (Default: `False`)

In order to deploy a basic setup of the _Greenlight_ frontend alongside _BigBlueButton_, the following variables can be set:

  * `bbb_greenlight_image`: Docker image to run for Greenlight (Default: `bigbluebutton/greenlight:v2`)
  * `bbb_greenlight_etcdir`: Path to configuration directory (Default: `/etc/bigbluebutton/greenlight`)
  * `bbb_greenlight_libdir`: Path to working directory (Default: `/var/lib/greenlight`)
  * `bbb_greenlight_dbdir`: Path to database directory (Default: Subdirectory `production` below `bbb_greenlight_libdir`)
  * `bbb_greenlight_logdir`: Path to log directory (Default: `/var/log/greenlight`)
  * `bbb_greenlight_redirect_root`: Whether to add a redirection from the domain root URL to Greenlight (Default: `false`)
  * `bbb_greenlight_db_adapter`: Database type to use (`sqlite3` or `postgresql`, default: `postgresql`)
  * `bbb_greenlight_db_host`: Name of database host. For `postgresql` adapter, special name `db` will spawn database in a separate container (Default: `db`)
  * `bbb_greenlight_db_username`: User name for database connection (Default: `postgres`)
  * `bbb_greenlight_db_name`: Name of Greenlight database (Default: `greenlight_production`)
  * `bbb_greenlight_db_port`: Host port for database connection (Default: `5432`)
  * `bbb_greenlight_environment`: Dictionary of additional Greenlight environment variables (Default: empty)

## Example Playbook

```
---
- hosts: bbb
  remote_user: ansible
  become: True
  become_user: root
  become_method: sudo
  gather_facts: True
  roles:
    - role: ansible-bigbluebutton
      bbb_server_name: bbb.example.com
      bbb_configure_nat: True
      bbb_install_demo: True
      bbb_install_check: True
      bbb_configure_ssl: True
      bbb_ssl_email: foo@bar.com

```
