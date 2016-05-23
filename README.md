# wt-sandbox

## 1 Overview

Simple sandbox to test WiredTiger. Contains two CentOS 7 hosts:

1. A server with the latest MongoDB Enterprise Advanced server installed and running

2. A client with the POCDriver installed and ready to run.

*Note*: For testing only! This is not production ready, nor is there any security enabled. Use at your own risk!

## 2 Prerequisites

* Mac OS X or Linux host
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) installed
* [Vagrant](https://www.vagrantup.com/downloads.html) installed
* Ansible installed ([Mac OSX](https://valdhaus.co/writings/ansible-mac-osx/) - _Hint_: Run "brew install ansible";  [Linux](http://docs.ansible.com/ansible/intro_installation.html))
* Host machine is connected to the internet (required only for the installation/configuration - once configured, you should be able to re-start the environment offline)
* 4GB-5GB free disk and 4.5GB RAM for the 2 guests (total)

## 3 Envrionment

Run:

    vagrant up

*Note*: The first time it can take a few minutes to complete. Be patient!

Starts two CentOS 7 hosts:

1. 'client', hostname: 'wtclient', 512MB RAM

2. 'server', hostname: 'wtserver', 4GB RAM + 2 x 2GB xfs-formatted non-sparse disks

Manage each independently e.g. 'vagrant up server' or 'vagrant halt client'

To shutdown guests:

    vagrant halt

To terminate & remove guest VMs + disks:

    vagrant destroy -f

### Client

Connecting:

    vagrant ssh client

[POCDriver](https://github.com/johnlpage/POCDriver) installed in /home/vagrant/POCDriver

Launch client application via:

    cd ~/POCDriver/bin
    java -jar POCDriver.jar -e -c mongodb://wtserver

Lots of other options, worth exploring!

    java -jar POCDriver.jar --help

### Server

Connecting:

    vagrant ssh server

Running latest MongoDB Enterprise Advanced server (currently 3.2.6)

Log file:

    /var/log/mongodb/mongod.log

Config file:

    /etc/mongod.conf

Initially set up to use MMAPv1 storage engine with dbpath set to /mnt/mmap/data

Config file contains additional commented out sections for:
- WiredTiger storage engine with dbpath set to /mnt/wt/data
- InMemory storage engine with dbpath set to /var/lib/mongo

Copyright (c) R Bohan 2016
