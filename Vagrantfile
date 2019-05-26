# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    config.vm.define "cicd" do |d|
      if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
        d.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
      else
        d.vm.synced_folder ".", "/vagrant"
      end
      d.vm.box = "ubuntu/bionic64"
      d.vm.hostname = "cicd"
      d.vm.network "private_network", ip: "10.100.198.200"
      d.disksize.size = "50GB"
      d.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]            
        v.memory = 3096
      end
      d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
      d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/acs.yml -i /vagrant/ansible/inventory/local -v"
    end
    config.vm.define "k8s-master" do |d|
      if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
        d.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
      else
        d.vm.synced_folder ".", "/vagrant"
      end
      d.vm.box = "ubuntu/bionic64"
      d.vm.hostname = "k8s-master"
      d.vm.network "private_network", ip: "10.100.199.200"
      d.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]     
        v.memory = 3096
        v.cpus = 2
      end
    end
    (1..4).each do |i|
      config.vm.define "k8s-node-#{i}" do |d|
        if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
          d.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
        else
          d.vm.synced_folder ".", "/vagrant"
        end
        d.vm.box = "ubuntu/bionic64"
        d.vm.hostname = "k8s-node-#{i}"
        d.vm.network "private_network", ip: "10.100.197.20#{i}"
        d.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]     
          v.memory = 3096
          v.cpus = 2
        end
      end
    end
    if Vagrant.has_plugin?("vagrant-cachier")
      config.cache.scope = :box
    end
    if Vagrant.has_plugin?("vagrant-vbguest")
      config.vbguest.auto_update = false
      config.vbguest.no_install = true
      config.vbguest.no_remote = true
    end
end