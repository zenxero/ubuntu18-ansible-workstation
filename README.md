# Archival Purposes

# WARNING - This playbook is outdated.

This an archive of an old Ansible playbook circa 2018 or so. It was written only for Ubuntu 18.04.

# Setup Ubuntu Workstation

This Ansible playbook will set up a basic Ubuntu workstation with the packages and configs that I need.

__NOTE:__ This playbook is configured to run against the local host only.

## Table of Contents
  * [Requirements](#requirements)
  * [Bootstrapping](#bootstrapping)
  * [Usage](#usage)
  * [Firefox Tweaks](#firefox-tweaks)
  * [Change Window Snapping Highlight Color](#window-highlight)

<a name="requirements"></a>
## Requirements

  * [Ansible Control System Requirements](https://docs.ansible.com/ansible/latest/intro_installation.html#control-machine-requirements)
  * An Ubuntu 18.04 system with the `ansible` and `git` packages installed.
  * Sudo permissions to run this playbook locally

<a name="bootstrapping"></a>
## Bootstrapping

1. In order to run this playbook, Ansible needs to be installed from the official PPA. To install it, run the following commands:

```bash
sudo add-apt-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
```

2. On your local system with ansible and git installed, clone this git repo.


<a name="usage"></a>
## Usage

First, take a look at the [setup-workstation.yml](setup-workstation.yml) file. This file contains all of the options for this playbook. Pick and choose the things that you would like to enable before running the playbook. Most options are a simple "True" or "False" selection.

1. It's a good idea to run a dry-run check before you actually run the playbook to make changes to your system. To perform a dry-run, run the following command:

```bash
ansible-playbook setup-workstation.yml --check
```

You will be asked for your your sudo password since this playbook needs root privileges for various tasks.

Enter your sudo password. You should see output like this (output shortened for readability):

```bash
SUDO password: 

PLAY [localhost] ***************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************
ok: [localhost]
......
......
......
......
PLAY RECAP *********************************************************************************************************************************
localhost                  : ok=17   changed=4    unreachable=0    failed=0
```

2. If there are no failures and you want to run it for real, just remove the `--check` from the end of the command:

```bash
ansible-playbook setup-workstation.yml
```

3. For verbose output, you can add up to 4 v's to the end of the command, like so:

```bash
ansible-playbook setup-workstation.yml -vvvv
````

<a name="firefox-tweaks"></a>
## Firefox Tweaks

### Theme override

With the default GTK dark theme and Firefox set to dark mode, sometimes text fields will be dark and the text will also be dark. This means that you can't see anything in the text fields.

What you need to do is use a theme like Arc, and then force Firefox to use that theme with an override.

```bash
sudo apt install arc-theme
```

To fix this, use this Firefox `about:config` setting:

```
widget.content.gtk-theme-override = Arc
```

### Firefox refresh rate

If your desktop compositor is set to a higher than 60 Hz refresh rate, such as 144 Hz, Firefox will not automatically display content at that refresh rate.

To fix this, use the Firefox `about:config` setting:

```
layout.frame_rate = 144
```

<a name="window-highlight"></a>
## Change Window Snapping Highlight Color

The default window snapping color in Ubuntu is transparent orange. This tends to clash with the themes that I use that are a more blue/teal color with dark mode.

To change the window snapping highlight color, edit the file:
```
/usr/share/gnome-shell/theme/ubuntu.css
```

Near line 645, there should be a section that looks like this:
```css
/* Tiled window previews */
.tile-preview {
  background-color: rgba(187, 62, 17, 0.5);
  border: 1px solid #dd4814; }
```

If you would like to make the hightlighting more teal, change the values to something like this:
```css
/* Tiled window previews */
.tile-preview {
  background-color: rgba(0, 128, 128, 0.5);
  border: 1px solid #008080; }
```

__NOTE:__ You will need to log out and log back in for these changes to take effect.
