# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.7.0"

unless Vagrant.has_plugin?("vagrant-bindfs")
  STDOUT.write "\nvagrant-bindfs is not installed! Please install using\n"
  STDOUT.write "$ vagrant plugin install vagrant-bindfs\n\n"
  exit
end

unless Vagrant.has_plugin?("vagrant-vbguest")
  STDOUT.write "\nvagrant-vbguest is not installed! Please install using\n"
  STDOUT.write "$ vagrant plugin install vagrant-vbguest\n\n"
  exit
end

if Vagrant::Util::Platform.windows?
  unless Vagrant.has_plugin?("vagrant-winnfsd")
    STDOUT.write "\nvagrant-winnfsd is not installed! Please install using\n"
    STDOUT.write "$ vagrant plugin install vagrant-winnfsd\n\n"
    exit
  end
end

Vagrant.configure(2) do |config|

  # Si se está utilizando una versión local de la máquina de vagrant,
  # desde el registro de vagrant o directamente desde la descarga,
  # comentar las siguientes 2 líneas y configurar las líneas siguientes

  config.vm.box = "halles/wcde"
  config.vm.box_version = "~> 0.0.20"

  # Nombre de la máquina local de vagrant
  # config.vm.box = "wcde-dev"

  # En caso de necesitar utilizar el box previamente descargado
  # usar esta línea para definir la ruta al archivo.
  # config.vm.url = "./maquina-local.box"

  config.vm.provider "virtualbox" do |v|
    v.name = "WCDE"
  end

  config.vm.box_check_update = true
  config.vbguest.auto_update = true

  config.vm.network "private_network", type: "dhcp"

  # Regular services ports

  config.vm.network "forwarded_port", guest: 80, host: 8080     # HTTP
  config.vm.network "forwarded_port", guest: 443, host: 8443    # HTTPS

  config.vm.network "forwarded_port", guest: 3306, host: 3306   # MySQL
  config.vm.network "forwarded_port", guest: 27017, host: 27017 # MongoDB

  config.vm.network "forwarded_port", guest: 8980, host: 8980   # phpMyAdmin
  config.vm.network "forwarded_port", guest: 8981, host: 8981   # Mongo Express
  
  for i in 9001..9010 # NodeJS or other services
    config.vm.network :forwarded_port, guest: i, host: i
  end

  if Vagrant::Util::Platform.windows?
    config.vm.synced_folder "../", "/temp-nfs-mounts/sites-unbinded", type: :nfs
    config.bindfs.bind_folder "/temp-nfs-mounts/sites-unbinded", "/sites", :force_user  => "vagrant", :force_group => "vagrant", :create_as_user => true
  else
    config.vm.synced_folder "../", "/temp-nfs-mounts/sites-unbinded", type: :nfs
    config.bindfs.bind_folder "/temp-nfs-mounts/sites-unbinded", "/sites", :force_user  => "vagrant", :force_group => "vagrant", :create_as_user => true
  end

end
