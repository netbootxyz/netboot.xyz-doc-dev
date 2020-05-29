---
title: Self Hosting
weight: 15
date: '2019-04-14T16:39:54.000Z'
description: How to self host your own netboot.xyz in your environment
---

# Self Hosting

netboot.xyz was originally a hosted only tool and used static source files that were difficult to customize. Now you can generate your own self hosted environment using the same tooling to generate the hosted site.

[Ansible](https://www.ansible.com/), an open-source automation engine, is utilized to generate custom templates based on a set of default configurations which can then be overridden by the user. This allows a user to customize a netboot.xyz environment to their specification and set up a PXE server easily. The Ansible playbooks will generate:

* Menus for their netboot.xyz environment using default configuration settings
* iPXE Bootloaders for booting into that environment
* Customized menu options for those who have additional options they want to add

## Role structure

The netbootxyz Ansible [role](https://github.com/netbootxyz/netboot.xyz/tree/master/roles/netbootxyz) is located in the main netboot.xyz repository. Most of the logic for netboot.xyz is contained in these areas:

* defaults/main.yml - Consists default settings for deployment, OS versions, Utilities, and Bootloaders
* tasks/\* - Contains all tasks for rendering templates and compiling iPXE bootloaders 
* templates/disks - Templates for iPXE bootloaders
* templates/menus - Templates for netboot.xyz menus
* vars/\* - Contain required package lists needed to support the compile and deployment of netboot.xyz

## Deploying using Ansible

To run a deployment using Ansible, first install Ansible, Apache and git:

```text
# For Debian/Ubuntu:
apt install -y ansible git apache2

# For Red Hat/CentOS/Fedora
yum install -y ansible git httpd
```

Then check out the netboot.xyz repo:

```text
git clone https://github.com/netbootxyz/netboot.xyz.git /opt/netboot.xyz
```

Finally run the Ansible playbook:

```text
cd /opt/netboot.xyz
ansible-playbook -i inventory site.yml
```

The output will be dropped into `/var/www/html` by default. You can override this to deploy to the web server server directory of your choice.

## Deploying with Docker

You can also leverage docker to generate the netboot.xyz menu and disks in a container which then outputs the results of the rendered templates and compiled iPXE disks into a directory. First ensure you have docker installed and then run:

```text
docker build -t localbuild -f Dockerfile-build .
docker run --rm -it -v $(pwd):/buildout localbuild
```

The build output will be in the generated folder `buildout`. Docker provides a consistent and isolated environment for generating the build output. From there you'd drop the files into the root of your favorite web server.

## Local Overrides

Ansible will handle source generation as well as iPXE disk generation with your settings. It will generate Legacy \(PCBIOS\) and UEFI iPXE disks that can be used to load into your netboot.xyz environment. If you want to override the defaults, you can put overrides in user\_overrides.yml. See `user_overrides.yml` for examples.

Using the overrides file, you can override all of the settings from the defaults/main.yml so that you can easily change the boot mirror URLs when the menus are rendered. If you prefer to do this after the fact, you can also edit the boot.cfg to make changes, but keep in mind those changes will not be saved when you redeploy the menu.

## Self Hosted Custom Options

In addition to being able to host netboot.xyz locally, you can also create your own custom templates for custom menus within netboot.xyz. Those templates are rendered during deployment and are available from the main menu via the custom menu option.

When these options are set:

```text
custom_generate_menus: true
custom_templates_dir: "{{ netbootxyz_conf_dir }}/custom"
```

The menu will add an option for custom menus and attempt to load into custom/custom.ipxe. From there custom options can be built and maintained separately from the netboot.xyz source tree so that both menus can be updated independently.

A sample menu is provided to demonstrate how to configure and set up a menu. You can copy the custom directory from the repo:

```text
cp etc/netbootxyz/custom /etc/netbootxyz/custom
```

