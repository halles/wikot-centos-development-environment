# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?("vagrant-bindfs")
  raise 'vagrant-bindfs is not installed! Please install with vagrant plugin install vagrant-bindfs'
end

unless Vagrant.has_plugin?("vagrant-vbguest")
  raise 'vagrant-bindfs is not installed! Please install with vagrant plugin install vagrant-vbguest'
end

Vagrant.configure(2) do |config|

  config.vm.box = "halles/wcde"
  config.vm.box_version = "~> 0.0.7"

  config.vm.provider "virtualbox" do |v|
    v.name = "WCDE"
  end

  config.vbguest.auto_update = true

  config.vm.synced_folder "./", "/vagrant", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777","fmode=666"]
  config.vm.synced_folder "../", "/sites", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777","fmode=666"]
  config.vm.network "private_network", type: "dhcp"

  # Regular services ports

  config.vm.network "forwarded_port", guest: 80, host: 8080   # HTTP
  config.vm.network "forwarded_port", guest: 443, host: 8443  # HTTPS
  config.vm.network "forwarded_port", guest: 8980, host: 8980 # phpMyAdmin on HTTP
  config.vm.network "forwarded_port", guest: 3306, host: 3306 # MySQL
  config.vm.network "forwarded_port", guest: 27017, host: 27017 # MongoDB

  # Extra ports for extra services: nodejs, ruby, etc.

  config.vm.network "forwarded_port", guest: 9000, host: 9000 
  #config.vm.network "forwarded_port", guest: 9001, host: 9001
  #config.vm.network "forwarded_port", guest: 9002, host: 9002
  # config.vm.network "forwarded_port", guest: 9003, host: 9003


end
