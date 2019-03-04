# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
      config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
    else
      config.vm.synced_folder ".", "/vagrant"
    end
    config.vm.define "cicd" do |d|
      d.vm.box = "ubuntu/bionic64"
      d.vm.hostname = "cicd"
      d.vm.network "private_network", ip: "10.100.198.200"
      d.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]            
        v.memory = 4096
      end
      d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
      d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/acs.yml -i /vagrant/ansible/inventory/local -v"
      d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/cicd.yml -i /vagrant/ansible/inventory/cicd -v"
    end
    config.vm.define "dev" do |d|
      if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
        config.vm.synced_folder "../app", "/app", mount_options: ["dmode=700,fmode=600"]
      else
        config.vm.synced_folder "../app", "/app"
      end
      d.vm.box = "ubuntu/bionic64"
      d.vm.hostname = "dev"
      d.vm.network "private_network", ip: "10.100.199.200"
      d.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]            
        v.memory = 4096
      end
      d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
      d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/acs.yml -i /vagrant/ansible/inventory/local -v"
      d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/dev.yml -i /vagrant/ansible/inventory/dev -v"
    end
end