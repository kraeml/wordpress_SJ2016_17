# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # vbguest-plugin installed? https://github.com/dotless-de/vagrant-vbguest
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end
  config.vm.box = "kraeml/xenial-64-de"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    #vb.gui = true
    # Customize the amount of memory,cpu on the VM:
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--cpus", 2]
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]

    vb.linked_clone = true
  end

  config.vm.define "db" do |db|
    db.vm.network :private_network, ip: "192.168.68.22"
    db.vm.hostname = "mysql.rdf.loc"
    #db.vm.provision :shell, :path => "myspl-skript/install-mysql.sh"
    db.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
    end
    config.vm.provider "virtualbox" do |vb|
      vb.name = "mysql-wp"
    end
  end

  config.vm.define "wp" do |wp|
    wp.vm.network :private_network, ip: "192.168.68.21"
    wp.vm.hostname = "wordpress.rdf.loc"
    #wp.vm.provision :shell, :path => "install-wp.sh"
    wp.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
    end
    config.vm.provider "virtualbox" do |vb|
      vb.name = "wordpress"
    end
  end
end
