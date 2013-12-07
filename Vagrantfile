# -*- mode: ruby -*-
# vi: set ft=ruby :

server_ip = '192.168.56.100'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Base box
  config.vm.box = "centos65_64"
  config.vm.box_url = "https://dl.dropboxusercontent.com/u/8525888/vagrant-boxes/centos65_64.box"

  # Network
  config.vm.hostname = "vagrant-gis"
  config.vm.network :private_network, ip: server_ip

  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    vb.gui = true
  
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.provision :shell, :inline => "service iptables stop && chkconfig iptables off"
  config.vm.provision :shell, :inline => "cd /tmp && wget -q http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm && rpm -Uvh epel-release-6-8.noarch.rpm; rm -f epel-release-6-8.noarch.rpm"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "chef-repo/cookbooks"
    chef.roles_path = "chef-repo/roles"
    
    chef.add_role "postgresql"
    chef.add_role "geoserver"
    chef.json = {
        'postgresql' => {
            'config' => {
                'listen_addresses' => server_ip
            }
        }
    }
  end

end
