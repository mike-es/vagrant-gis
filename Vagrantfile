# -*- mode: ruby -*-
# vi: set ft=ruby :

server_ip = '192.168.56.100'

Vagrant.configure("2") do |config|
  config.vm.box = "centos65_64"
  config.vm.box_url = "https://dl.dropboxusercontent.com/u/8525888/vagrant-boxes/centos65_64.box"
  config.vm.hostname = "vagrant-gis"
  config.vm.network :private_network, ip: server_ip

  config.vm.provider :virtualbox do |vb|
    vb.name = "vagrant-gis"
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  
  # Disable firewall for development
  config.vm.provision :shell, :inline => "service iptables stop && chkconfig iptables off"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "chef-repo/cookbooks"
    chef.roles_path = "chef-repo/roles"
    chef.add_role "postgis"
    chef.add_role "geoserver"
    chef.json = {}
  end

end
