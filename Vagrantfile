# -*- mode: ruby -*-
# vi: set ft=ruby :

machines = [
  { :name => "srv0", :alias => "cloud", :ip => ".10", :box => "generic/ubuntu1804", :cpus => 1, :memory => 2048 },
  { :name => "srv1", :alias => "chat", :ip => ".11", :box => "generic/ubuntu1804", :cpus => 1, :memory => 2048 },
  { :name => "srv2", :alias => "learn", :ip => ".12", :box => "generic/ubuntu1804", :cpus => 1, :memory => 2048 },
  { :name => "srv3", :alias => "wiki", :ip => ".13", :box => "generic/ubuntu1804", :cpus => 1, :memory => 2048 }
]

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  machines.each_with_index do |machine, index|
    config.vm.define machine[:name] do |subconfig|
      subconfig.vm.box = machine[:box]
      subconfig.vm.network "private_network", ip: "192.168.121" + machine[:ip]
      subconfig.vm.hostname = machine[:name] + ".mbconcept.internal"
      subconfig.hostsupdater.aliases = [machine[:alias] + ".mbconcept.internal"]
      subconfig.vm.provider "libvirt" do |lv|
        lv.cpus = machine[:cpus]
        lv.memory = machine[:memory]
      end
      subconfig.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.cpus = machine[:cpus]
        vb.memory = machine[:memory]
      end
      if index == machines.size - 1
        subconfig.vm.provision "ansible" do |ansible|
          ansible.galaxy_role_file = "requirements.yml"
          ansible.galaxy_command = "ansible-galaxy collection install --requirements-file=%{role_file} --force"
          ansible.inventory_path = "inventories/vagrant.yml"
          ansible.playbook = "playbook.yml"
          ansible.limit = "all"
          ansible.extra_vars = "@vaults/local.yml"
        end
      end
    end
  end
end
