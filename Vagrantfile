# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|  
  config.vm.define "deploy" do |deploy|
      deploy.vm.box = "ubuntu/trusty64"

      deploy.vm.network "private_network", ip: "192.168.221.100"
      deploy.vm.provision "shell" , inline: <<-SHELL
        wget http://mirrors.163.com/.help/sources.list.trusty -O /etc/apt/sources.list
        apt-get update
        add-apt-repository ppa:ansible/ansible
        apt-get update
        apt-get install -y ansible
        cat >> /etc/ssh/ssh_config <<EOF   
      StrictHostKeyChecking no
      UserKnownHostsFile=/dev/null
EOF
    SHELL

    deploy.vm.provider "virtualbox" do |vbox|
      vbox.memory = 1024
    end
  end

  config.vm.define :controller do |controller|
    controller.vm.box = "ubuntu/xenial64"
    controller.vm.hostname = "controller"
    controller.vm.network :private_network, ip: "192.168.221.101" # eth1 internal
    controller.vm.network :public_network, bridge: "eth1" ,ip: "192.168.1.241" # eth2 external
    controller.vm.provider "virtualbox" do |vbox|
      vbox.customize ["modifyvm", :id, "--memory", "4098"]
      vbox.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"] # eth2
    end
  end

  config.vm.define :compute do |compute|
    compute.vm.box = "ubuntu/xenial64"
    compute.vm.hostname = "compute"
    compute.vm.network :private_network, ip: "192.168.221.102" # eth1 internal
    compute.vm.network :public_network, bridge: "eth1" ,ip: "192.168.1.242" # eth2 external
    compute.vm.provider "virtualbox" do |vbox|
      vbox.customize ["modifyvm", :id, "--memory", "2048"]
      vbox.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"] # eth2
    end
  end
end
