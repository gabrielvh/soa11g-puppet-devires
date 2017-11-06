# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "soaapp" , primary: true do |soaapp|
    soaapp.vm.box = "centos-6.5-x86_64"
    soaapp.vm.box_url = "https://dl.dropboxusercontent.com/s/np39xdpw05wfmv4/centos-6.5-x86_64.box"

    soaapp.vm.hostname = "soaapp.devires.com"

    soaapp.vm.synced_folder "."                    , "/vagrant", :mount_options => ["dmode=777","fmode=777"]
    soaapp.vm.synced_folder "/tmp/software", "/software"

    soaapp.vm.network :private_network, ip: "10.10.10.10"

    soaapp.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "3500"]
      vb.customize ["modifyvm", :id, "--name"  , "soaapp"]
    end

    soaapp.vm.provision :shell, :inline => "ln -sf /vagrant/puppet/hiera.yaml /etc/puppet/hiera.yaml"

    soaapp.vm.provision :puppet do |puppet|
      puppet.manifests_path    = "puppet/manifests"
      puppet.module_path       = "puppet/modules"
      puppet.manifest_file     = "soaapp.pp"
      puppet.options           = "--verbose --hiera_config /vagrant/puppet/hiera.yaml"

      puppet.facter = {
        "environment"                     => "development",
        "vm_type"                         => "vagrant",
      }

    end

  end

end
