# Trystack playbook

## Introduction
This playbook is used to create an instance on trystack to be used for testing purposes only
for more information on trystack visit http://trystack.org

It also adds a new entry to `/etc/hosts` with the name `trystack` and the floating ip which was associated with the instance.

**HINT** There is a bug which allows multiple trystack entries to be added, will be fixed soon.

## Requirements
This playbook requires you to setup the trystack credentials used to access trystack API for creating instances.
Create the file `~/.config/openstack/clouds.yaml` with the following content:
```
  ---
  clouds:
    trystack:
      auth:
        auth_url: <URL>
        username: <USERNAME>
        password: <PASSWORD>
        project_name: <PROJECT>
      region_name: <REGION>
      dns_api_version: 1
```
Replace everything between <> with your own values retrieved from: https://x86.trystack.org/dashboard/project/access_and_security/api_access/openrc/
This link will download a file which contains values for all the options above except for password
to get the password visit: https://x86.trystack.org/dashboard/settings/apipassword/ and click on Request API password.

You need to create a keypair locally using `ssh-keygen` to be uplaoded to trystack and used when creating the instance.

This playbook installs all its requirements by itself however it is recommended to enable passwordless
sudo on your desktop/laptop to run it smoothly, otherwise run ansible with `-K` option.

## variables
There are three variables which can be changed in this playbook:
* `ins` It takes one of two values `c` to create an instance and `d` to delete it, default is `c`.
* `ins_name` The name of the instance, can be any valid string, default is `test_server`
* `image_name` The image name used to create the instance possible values are `CentOS6`, `CentOS7`, `CentOS7-Atomic`, `Cirros-0.3.4`, `CoreOS`, `Fedora23`, `Fedora24`, `Fedora25 Atomic`, `Ubuntu14.04`, `Ubuntu16.04`, `openSUSE13.2`, default is `Ubuntu16.04`

## Examples
**HINT** if you did not configure passwordless sudo use `-K` option with all these commands
* To create an instance called `test_server` with Ubuntu 16.04 installed on it run `ansible-playbook trystack_instance.yml`
* To create an instance called `hello_trystack` with Ubuntu14.04 installed on it run `ansible-playbook trystack_instance.yml -e ins_name=hello_trystack -e image_name=Ubuntu14.04`
* To delete ans instance called `test_del` run `ansible-playbook trystack_instance.yml -e ins=d`

## Hints
Sometimes the playbook fails with the following message `(409) Client Error for url: http://8.43.86.2:9696/v2.0/floatingips.json Quota exceeded for resources: ['floatingip']"`
this error is not related to the playbook at all it means that there is a problem with trystack service
since it is a free service it sometimes suffers from errors, be patient.
