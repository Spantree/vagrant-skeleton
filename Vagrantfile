# -*- mode: ruby -*-
# vi: set ft=ruby :

# Change this to match the root project/folder name
PROJECT_NAME = "project"
PROJECT_ROOT = "/usr/local/src/#{PROJECT_NAME}"

Vagrant.configure('2') do |config|
  config.vm.box = 'base-precise-64'

  # Use this if connecting locally from the Spantree network
  # config.vm.box_url = 'http://10.0.1.54/estalk-precise-vbox-x86_64.box'

  # Use this if connecting from the outside internet
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  config.vm.synced_folder '.', PROJECT_ROOT, :create => 'true'

  config.vbguest.auto_update = true
  config.vbguest.no_remote = false

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.hostname = 'test.local'

  config.vm.provider :virtualbox do |v, override|
    override.vm.network :private_network, ip: '192.168.80.100'
    v.customize ['modifyvm', :id, '--memory', 768]
  end

  config.vm.provision :shell, :path => 'shell/initial-setup.sh', :args => '/vagrant/shell'
  config.vm.provision :shell, :path => 'shell/update-puppet.sh', :args => '/vagrant/shell'
  config.vm.provision :shell, :path => 'shell/librarian-puppet-vagrant.sh', :args => '/vagrant/shell'

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = 'puppet/manifests'
    puppet.facter = {
      "project_root" => PROJECT_ROOT
    }
    puppet.options = [
      '--verbose',
      '--debug',
      "--modulepath=/etc/puppet/modules:${PROJECT_ROOT}/puppet/modules"
    ]
  end
end
