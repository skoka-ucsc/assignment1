# Dev Environment for REDIS

This git repo provides a dev environment to build and test [REDIS](https://github.com/redis)
software using Vagrant.

## Pre-requistises
- [Vagrant](https://www.vagrantup.com) needs to be installed.
- A backing provider for Vagrant such as [VirtualBox](https://www.virtualbox.org) or [Hyper-V](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) needs to be installed.
- The [redis git repo](https://github.com/redis/redis) needs to be cloned onto a local folder.

## User can provide and update dev environment configuration using config.yml file
- redis_folder : folder path for location of the cloned redis git repository.
- vagrant_provider : backing provider for Vagrant, default value is virtualbox.
- redis_port: host port on which redis server is accessible, default value is 9001.

## Instructions to use the dev environment
1. Run `vagrant up` command to bring up the vagrant box.
2. Run `vagrant ssh` command to ssh into the vagrant box.
3. Run `cd /redis` command to change directory to the folder where the cloned redis git repository exists.
4. Run `.make distclean` command clean previous build remnants.
5. Run `./make` command to build redis software.
6. Run `cd src;./redis-server` command to run the redis server.
7. Run `vagrant halt` command to shut down the vagrant box.

## Useful links
- [Vagrant Command-Line Interface](https://developer.hashicorp.com/vagrant/docs/cli)
- [GitHub Command-Line Docs](https://docs.github.com/en/github-cli)
