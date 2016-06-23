# devbox #
This is a Vagrant specification for setting up a virtual machine to develop with Aja (new continuous delivery tool) and IEC tools.

Vagrant is CLI for VirtualBox (or any other virtualization software) that also provides pre-built images ... it's important to know that those two are separate software products which usually means that Vagrant can be a step behind because the heavy lifting is done by VirtualBox

## Bug reporting & Support
If you find issues with the vagrant box or some problems, then please use the issues tab. Or you can join the Devbox chat in HipChat

## Benefits

  * All Vaimo developer tools are included, nothing needs to be installed on your machine directly.
  * Development environment is the same as our servers, same os, same setup and configuration + additional tools.
  * More room for experimentation. **The whole machine can be rolled back to its initial state.**
  * Gives better understanding how to manage different aspects of the machine in the ACTUAL hosting environment.
  * Easy to start developing on a new machine (easy to get all the tools installed - and uninstalled when needed)
  * Easy to share new tools/setup with all developers in the team.
  * Everything can be installed/reinstalled in minutes - just destroy the old VM and start a new one.
  * Good for offline sales-demos - when you're done with it, just remove the VM and your machine is clean from unwanted software.
  * **You can create custom shell scripts under custom/shell folder that are loaded after provision into your VM**

## Tools Included

  * IEC Tools (**installed by default**)
  * Aja (**installed by default**)
  * Composer (**installed by default**)
  * Percona Server (MySQL), Apache, PHP (**installed by default**)
  * XDebug (**installed and configured by default**)
  * Python, PIP
  * PHPUnit, CasperJS, PhantomJS, Codeception, Karma
  * IonCube loader php module
  * Node.js (With grunt-cli and gulp added)
  * SOLR 4.10 (On port 4444) with a core (called 'magento') for Magento (SOLR 4.X requires vaimo/solraddons to run properly)
  * Sphinx documentation generator
  * Custom prompt (disabled by default)

Note that by default, the installation of many of these tools will be skipped. See more on this under "Configuration".

## Instructions

### Prerequisites

* You need Vagrant
* You need VirtualBox

Recommended versions (Which have been tested to work together):

* Vagrant (Tested on 1.7.4, don't use 1.7.3) (https://www.vagrantup.com/downloads.html)
* Virtual Box (Tested on 4.3.x) (https://www.virtualbox.org/wiki/Downloads)

### Installation

1. Run: `$ hg clone ssh://hg@bitbucket.org/vaimo/devbox`
2. Then `$ cd devbox`
3. **[Only for developers]** Setup your HOST (Your mac) according to **Suggestions on HOST/VM configuration**.
4. Then `$ vagrant up` (Now vagrant will download the box, then install everything. Takes a while.)
5. Open up devbox.vaimo.dev in a web browser. The page should say "It works!".

### Optional extras

* For taking snapshots of the environment: `$ vagrant plugin install vagrant-vbox-snapshot`. This will add a command `vagrant snapshot`
* For better permissions management of NFS synchronized sites root, install bindfs: `$ vagrant plugin install vagrant-bindfs` (NOTE: using this will have a huge performance hit, site loads 50% slower).

-----------

# For developers

Following sections are only relevant to developers.

## Terminology

* **VM, GUEST** - Virtual Machine. The machine that gets created when you run `$ vagrant up` and gets destroyed when you run `$ vagrant destroy`
* **HOST** - Your Mac OS X. The machine that has Vagrant (as software) installed and has the vaimo/devbox repository. Your development machine
* **SITES ROOT, PROJECT ROOT** - The folder on the HOST that has the folder where developer usually keeps all imported projects/sites. This is the folder you want to access with Mac's Finder.
* **NFS, SYNCHED SITES, SYNCHRONIZED FOLDER** - Used for sharing files between HOST and GUEST.
* **TRACKED FILES** - files that are part of devbox repository.
* **Puppet** - Framework that is used to describe what gets installed on the VM and how it's set up. Puppet is used by the hosting team to manage our actual live servers so this VM uses the same framework/files.

![devbox general schematics](https://confluence.vaimo.com/download/attachments/19106259/devbox.png)

## Suggestions on HOST/VM configuration

**==> HOST machine .aja configuration**

If you want the Aja user credentials to survive the up/destroy cycle of your machine, add the 
credentials to your mac's ~/.aja (note that you don't have to have aja installed on your Mac 
for this).

## Connecting with SequelPro

Use following configuration:

```
MySQL Host: 127.0.0.1
Username: root
Password: <get it by doing cat ~/.my.cnf inside the MV>
SSH Host: devbox (if you use custom host name via vm.name in user.yaml, use that one here)
SSH User: <leave blank>
SSH Password: <leave blank>

```

## Configuration
To start changing the guest machine to your liking you need to copy the file config/default.yaml to config/user.yaml 

If you know yaml then you can remove all the other extra addison's there that you don't want to change and leave only the ones you want to. **This means that you don't have to copy the whole .yaml file to user.yaml**

Here are some small explanations for the configuration options that you can change. There are more options in the file but unless you know what your doing dont change them:

---
* iec:
    * install: true - Install IEC Tools to the VM (true/false)
    * ssh_via_vpn: 0 - Use VPN enabled option in the VM (0/1)
* aja:
    * install: true - Install AJA Tools to the VM
    * enable_jira_hook: false - Enable force hira ticket number in AJA projects? (true/false)
* bash_prompt:
    * install: false - Install Bash Promt to the VM (false/true)
    * prompt_features: '' - What bash promt features should be enabled by default?
* ssp:
    * install: false - Install utility search serverportal to the VM (false/true)
* solr:
    * install: false - Install SOLR to the VM? (false/true)
    * version: 4.10.4 - What version of SOLR should we install?
* varnish:
    * install: false - Install varnish service
    * vcl_file: /absolute/path/to/varnish.vcl file
* ioncube:
    * install: false - Install IonCube php extentsion to the VM? (false/true)
* phantomjs:
    * install: true - Install PhantomJS to the VM?  (false/true)
* casperjs:
    * install: true - Install CastepJS to the VM? (false/true)
* phpunit:
    * install: true - Install PHPUnit component to the VM? (false/true)
* java:
    * install: false - Install JAVA for the VM? Enabled if SOLR is installed (false/true)
* magerun:
    * install: false - Install Magerun and Magerun2 for the VM? (false/true)
* sphinx:
    * install: false - Install Sphinx to the VM (false/true)
* xdebug:
    * install: true - Install XDebug and enable it in the VM? (true/false)
* mailhog:
    * install: false - Install Mailhog and enable it in the VM? (true/false)
* go:
    * install: false - Install Go Lang for the VM?  (false/true)
* codeception:
    * install: false - Install Codeception for the VM?  (false/true)
    * version: 1.8.7 - Codeception Version to install 
* node:
    * install: false - Install NodeJS to the VM?  (false/true)
* karma:
    * install: false - Install Karma to the VM? (false/true)
* vm:
    * release: precise - What distro of Ubuntu you woud like to have? (trusty (14.04) or precise (12.04))
    * name: devbox - Name for your VM? will be part of the main hostname for exmaple (devbox.vaimo.dev)
    * ip: 172.16.32.150 - IP For the VM
    * memory: 2024 - Memory size for the VM
    * cpu: 2 - How many CPU's to use?
    * sync_sites: true - Should we syn the WWW/Project folder to the VM?
    * sync_mount_opts: '' - Extra NFS mount options for sites folder (each item should be added as **yaml sequence**)
    * www_root: /private/var/www - WWW/Project path
    * enable_dev_mode: true - Enable Magento development mode & more verbose php error reporting
    * package_generate_mode: false - For advanced users. Used to generate new base-image based on VM puppep scripts
---

## Troubleshooting

If gulp, grunt or any other node.js related stuff startes acting all weird suddenly, ask yourself: “Did I do a vagrant provsion before all this started?” If the answer is yes, check that provision didn’t update node.js to a newer (and in your case unsupported) version. 
```
$ node -v
```

To fix this, change node.js version setting from “stable” to your earlier version in config.default.yaml or user.yaml
# general
