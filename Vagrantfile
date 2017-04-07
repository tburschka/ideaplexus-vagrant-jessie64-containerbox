# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

BOX        = "debian/jessie64"
BOX_MEMORY = 1024

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # define box
  config.vm.box = BOX
  config.vm.box_check_update = true

  # define ssh options
  config.ssh.insert_key = false

  # modify box configuration
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", BOX_MEMORY]
  end

  # set auto_update to false, if you do NOT want to check the correct
  # additions version when booting this machine
  config.vbguest.auto_update = true

  # do NOT download the iso file from a webserver
  config.vbguest.no_remote = false

  # prevent Vagrant from mounting the default /vagrant synced folder
  config.vm.synced_folder '.', '/vagrant', disabled: true

  # mount provisioning scripts and files
  config.vm.synced_folder "./provisioning", "/provisioning", owner: "vagrant", group: "vagrant"

  # required scripts
  config.vm.provision :shell, path: "./provisioning/scripts/ssh.sh", name: "install ssh packages"
  config.vm.provision :shell, path: "./provisioning/scripts/update-upgrade.sh", name: "upgrade packages"
  config.vm.provision :shell, path: "./provisioning/scripts/common-configs.sh", name: "set common configurations"
  config.vm.provision :shell, path: "./provisioning/scripts/common-packages.sh", name: "install common packages"
  config.vm.provision :shell, path: "./provisioning/scripts/locale.sh", name: "set locale"

  # run installation
  config.vm.provision :shell, path: "./provisioning/scripts/docker.sh", name: "install packages"
  config.vm.provision :shell, path: "./provisioning/scripts/docker-compose.sh", name: "install docker-compose"

  # clean up
  config.vm.provision :shell, path: "./provisioning/scripts/cleanup.sh", name: "cleanup box"

end
