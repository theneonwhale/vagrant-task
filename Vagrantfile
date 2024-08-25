# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_deps = <<-'SHELL'
#!/bin/bash
sudo apt-get update
sudo apt-get install -y openjdk-17-jdk
SHELL

Vagrant.configure("2") do |config|
  config.vm.box = "trc/ubuntu24.04-server-arm64"
  config.vm.box_version = "01.0"
  config.vm.box_architecture = "arm64"

  config.vm.network "private_network", ip: "192.168.100.100"

  config.vm.provision "shell", inline: $install_deps

  config.vm.provider "vmware_fusion" do |v|
    v.memory = 4096
    v.cpus = 2
  end

  config.trigger.before :up do
    config.vm.provision "shell", inline: <<-SHELL
      cd /vagrant
      ./gradlew bootRun
    SHELL
  end
end
