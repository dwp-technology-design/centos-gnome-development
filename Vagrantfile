# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
    config.vm.box           = "centos-gnome"
    config.vm.network       "private_network", ip: "192.168.56.33"

    config.vm.provider :virtualbox do |vb|
        vb.name = "dwp-development-virtual-machine"
        vb.cpus = 2
        vb.memory = "4096"
        vb.gui = true
        vb.customize ["modifyvm", :id, "--vram", "64"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end

    config.vm.provision :ansible do |ansible|
        ansible.playbook = 'default.playbook'
    end

    config.vm.provision "file", source: "~/.ssh/id_rsa.vagrant", destination: "/home/vagrant/.ssh/id_rsa"
    config.vm.provision "file", source: "~/.ssh/id_rsa.vagrant.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"

    config.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.vagrant.pub").first.strip
        s.inline = <<-SHELL
          echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
        SHELL
    end
end
