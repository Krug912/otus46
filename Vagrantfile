# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :node1 => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "node1",
        :memory => 1024,
        :net => [
                   ["192.168.255.250",  2, "255.255.255.0",  "lan"],
                   ["192.168.50.250",  8, "255.255.255.0"],
                ],
        :synced_folder => "./data/db"
  },

  :node2 => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "node2",
        :memory => 1024,
        :net => [
                   ["192.168.255.251",  2, "255.255.255.0",  "lan"],
                   ["192.168.50.251",  8, "255.255.255.0"],
                ]
  },

  :barman => {
        :box_name => "bento/ubuntu-22.04",
        :vm_name => "barman",
        :memory => 1024,
        :net => [
                   ["192.168.255.252",  2, "255.255.255.0",  "lan"],
                   ["192.168.50.252",  8, "255.255.255.0"],
                ]
  },
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]

      box.vm.provider "virtualbox" do |v|
#        v.memory = boxconfig[:vm_memory]
        v.cpus = 2
      end

      boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end

      if boxconfig.key?(:synced_folder)
         box.vm.synced_folder boxconfig[:synced_folder], "/servise/data/"
      end

      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
      SHELL
    end
  end
end
