# -*- mode: ruby -*-
# vi: set ft=ruby :

server_ip = '192.168.56.100'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "centos65_64"
  config.vm.box_url = "https://dl.dropboxusercontent.com/u/8525888/vagrant-boxes/centos65_64.box"

  config.vm.hostname = "vagrant-gis"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: server_ip

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  config.vm.provision :shell, :inline => "service iptables stop && chkconfig iptables off"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "chef-repo/cookbooks"
    chef.roles_path = "chef-repo/roles"
    #chef.data_bags_path = "../my-recipes/data_bags"
    
    chef.add_recipe "postgresql::server"
    chef.add_recipe "postgresql::contrib"
    chef.json = {
        'postgresql' => {
            'enable_pgdg_yum' => true,
            'version' => '9.3',
            'dir' => '/var/lib/pgsql/9.3/data',
            'password' => {
                'postgres' => '3175bce1d3201d16594cebf9d7eb3f9d'
            },
            'config' => {
                'listen_addresses' => server_ip
            },  
            'pg_hba' => [
                {
                    'type' => 'host',
                    'db' => 'all',
                    'user' => 'all',
                    'addr' => '192.168.56.0/24',
                    'method' => 'trust'
                }
            ],
            'client' => {
                'packages' => ['postgresql93']
            },
            'server' => {
                'packages' => ['postgresql93-server'],
                'service_name' => 'postgresql-9.3'
            },
            'contrib' => {
                'packages' => ['postgresql93-contrib']
            }
        }
    }
  end

end
