# -*- mode: ruby -*-
# vi: set ft=ruby :

# Define a shell script to install OpenJDK 17. This script will be used in the provisioning process.
$install_deps = <<-'SHELL'
#!/bin/bash
# Update the package list
sudo apt-get update
# Install OpenJDK 17 package
sudo apt-get install -y openjdk-17-jdk
SHELL

# Configure Vagrant
Vagrant.configure("2") do |config|

  # Specify the base box to use for the VM, including the box name and version.
  # This example uses an ARM64 architecture box.
  config.vm.box = "trc/ubuntu24.04-server-arm64"
  config.vm.box_version = "01.0"
  config.vm.box_architecture = "arm64"

  # Configure a private network for the VM with a specific IP address.
  # This allows the VM to be accessible on this private IP within the network.
  config.vm.network "private_network", ip: "192.168.100.100"

  # Provision the VM with the defined shell script to install OpenJDK 17.
  # This script will run when `vagrant up` is executed.
  config.vm.provision "shell", inline: $install_deps

  # Configure provider-specific settings for VMware Fusion.
  # Set the memory and number of CPUs allocated to the VM.
  config.vm.provider "vmware_fusion" do |v|
    v.memory = 4096  # Allocate 4 GB of RAM
    v.cpus = 2       # Allocate 2 CPUs
  end

  # Configure a trigger to run a script before the VM is fully brought up.
  # This script will change into the /vagrant directory and run the `./gradlew bootRun` command.
  config.trigger.before :up do
    config.vm.provision "shell", inline: <<-SHELL
      # Change to the /vagrant directory where the shared folder is mounted
      cd /vagrant
      # Run the Gradle bootRun task
      ./gradlew bootRun
    SHELL
  end
end
