# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # box configuration.  Ubuntu 12.04 LTS (Precise Pangolin; 32-bit)
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  # setup port forwarding to respond to HTTP requests.
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 443, host: 8081

  # TODO: Configure shared folders
  # config.vm.synced_folder "../data", "/vagrant_data"

  # configure ssh parameters
  config.ssh.max_tries = 40
  config.ssh.timeout   = 120

  # configure VirtualBox specific settings
  config.vm.provider :virtualbox do |vb|
    # enable the DNS proxy in NAT mode
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  # Enabling the Berkshelf plugin.
  config.berkshelf.enabled = true

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to exclusively install and copy to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  config.vm.provision :chef_solo do |chef|
    chef.json = {
      bundler: {
        apps_path: '',
        app: '/vagrant'  
      },
      rvm: {
        default_ruby: 'ruby-1.9.3',
        global_gems: [
          { name: 'bundler' },
          { name: 'rake' },
          { name: 'rubygems-bundler' },
          { 
            name: 'rails',
            version: '3.2.13'
          }
        ]
      }
    }

    chef.run_list = [
        'recipe[apt]',
        'recipe[build-essential]',
        'recipe[rvm::system]',
        'recipe[rvm::vagrant]',
        'recipe[bundler]',
        'recipe[bundler::install]'
    ]
  end
end
