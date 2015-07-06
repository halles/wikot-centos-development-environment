# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?("vagrant-vbguest")
  raise 'vagrant-bindfs is not installed! Please install with vagrant plugin install vagrant-vbguest'
end

Vagrant.configure(2) do |config|

  config.vm.box = "halles/wcde"
  config.vm.box_version = "~> 0.0.7"

  config.vm.provider "virtualbox" do |v|
    v.name = "WCDE"
  end

  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 443, host: 8443
  config.vm.network "forwarded_port", guest: 8980, host: 8980
  config.vm.network "forwarded_port", guest: 3306, host: 3306
  config.vbguest.auto_update = true

  config.vm.synced_folder "./", "/vagrant", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777","fmode=666"]
  config.vm.synced_folder "../", "/sites", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777","fmode=666"]

end
