# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "services-host" do |subconfig|
    subconfig.vm.box = "generic/ubuntu1804"
    subconfig.vm.hostname = "services-host.localdomain"
    subconfig.vm.network "forwarded_port", guest: 80, host: 80
    subconfig.vm.network "forwarded_port", guest: 443, host: 443
    subconfig.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: ".git/"
    subconfig.vm.provider "libvirt" do |lv|
      lv.cpus = 2
      lv.memory = 4096
    end
    subconfig.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.cpus = 2
      vb.memory = 4096
    end
    subconfig.vm.provider "hyperv" do |hv|
      hv.cpus = 2
      hv.memory = 4096
    end
    subconfig.vm.provider "vmware_desktop" do |vw|
      vw.gui = false
      vw.cpus = 2
      vw.memory = 4096
    end
    config.vm.provision "Install Ansible", type: "shell", inline: <<-SHELL
      which pip3 &>/dev/null || apt-get update && apt-get install -y python3-pip
      for package in ansible passlib bcrypt; do
        pip3 list --format=legacy | grep "${package}" &>/dev/null || pip3 install "${package}"
      done
    SHELL
    subconfig.vm.provision "Provision with Ansible", type: "ansible_local" do |ansible|
      ansible.galaxy_role_file = "requirements.yml"
      ansible.galaxy_command = "ansible-galaxy collection install --requirements-file=%{role_file} --force"
      ansible.inventory_path = "inventories/vagrant.yml"
      ansible.playbook = "playbook.yml"
      ansible.limit = "all"
      ansible.extra_vars = "@vaults/vagrant.yml"
    end
  end
end
