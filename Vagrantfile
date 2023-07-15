# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
require 'io/console'

# Check if the contents of the Vagrantfile have changed in the git repo.
# If yes, inform the user that the contents of Vagrantfile have changed
# and provide an option to the user to exit the program.
file_changed = `git diff Vagrantfile`
unless file_changed.empty?
  print "The contents of the Vagrantfile have changed in the git repo.\n"
  print "Do you want to continue running with older Vagrantfile? (y/n)\n"
  input = STDIN.getch
  print input
  if input == 'n' or input == 'N'
    print "\nExiting..."
    exit
  end
end

# Get the dev environment configuration from config.yml file
cur_dir = File.dirname(File.expand_path(__FILE__))
configs = YAML.load_file("#{cur_dir}/config.yml")
vagrant_config = configs['configs']

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "hashicorp/bionic64"
  config.vm.hostname = "redisdev"
  #config.vm.define = "redisdev"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  config.vm.network "forwarded_port", guest: 6379, host: 9001

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder vagrant_config['redis_folder'], "/redis"

  # Disable the default share of the current code directory. Doing this
  # provides improved isolation between the vagrant box and your host
  # by making sure your Vagrantfile isn't accessable to the vagrant box.
  # If you use this you may want to enable additional shared subfolders as
  # shown above.
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
      vb.name = "redisdev"
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get install -y build-essential
     apt-get install -y libsystemd-dev
     apt-get install -y pkg-config
     apt-get install -y tcl
     sysctl vm.overcommit_memory=1
     sysctl -w net.core.somaxconn=1024
     sysctl -w fs.file-max=500000
  SHELL
end
