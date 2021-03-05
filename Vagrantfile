# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "fedora/33-cloud-base"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

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
  config.vm.synced_folder "./guest", "/vagrant", create: true, owner: 'vagrant', disabled: false, type: 'virtualbox'

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
    # Customize the amount of memory on the VM:
    vb.memory = "8192"
    # Customize CPU cap
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "70"]
    # Customize number of CPU
    vb.cpus = 6
    # Customize VM name
    vb.name = "provbpf-vagrant"
    # Disable usb 2.0
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "off"]
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  config.trigger.after :destroy do |trigger|
    trigger.name = "Clean-up"
    trigger.info = "Cleaning things up!"
    trigger.ruby do |env,machine|
      require 'fileutils'
      FileUtils.rm_rf('guest')
    end
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    sudo dnf -y -v upgrade
    sudo dnf -y -v groupinstall 'Development Tools'
    sudo dnf -y -v install fedpkg fedora-packager rpmdevtools ncurses-devel
    sudo dnf -y -v install cmake clang gcc-c++ wget git pesign grubby
    sudo dnf -y -v install openssl-devel bc nano patch mosquitto sparse
    sudo dnf -y -v install flawfinder elfutils-libelf-devel
    sudo dnf -y -v install bison flex
    sudo dnf -y -v install uthash-devel
    sudo dnf -y -v install inih-devel
    sudo dnf -y -v install paho-c-devel
    sudo dnf -y -v install uncrustify
    sudo dnf -y -v install rpm-build redhat-rpm-config
    sudo dnf -y -v install coccinelle
    sudo dnf -y -v install phoronix-test-suite
    sudo dnf -y -v install automake
    sudo dnf -y -v install alien
    sudo dnf -y -v install dwarves
    sudo dnf -y -v install python3.8 python3-devel
    sudo dnf -y -v install llvm bpftool libbpf libbpf-devel elfutils-libelf
    sudo dnf -y -v install libcap
    sudo dnf -y -v install openssl
    sudo dnf -y -v install valgrind
    sudo dnf -y -v install binutils-devel
    sudo dnf -y -v install readline-devel
    sudo dnf -y -v install libcap-devel
    sudo dnf -y -v install ruby

    # make it so that /tmp is cleaned on reboot
    touch /etc/tmpfiles.d/boot.conf
    echo 'R! /tmp 1777 root root ~0' > /etc/tmpfiles.d/boot.conf
    echo '*                -       memlock         unlimited' >> /etc/security/limits.conf
  SHELL
end
