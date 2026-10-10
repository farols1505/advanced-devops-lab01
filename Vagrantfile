# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrant only creates the machines. There is deliberately no provisioner:
# everything after `vagrant up` is done by running ansible-playbook against
# the static inventory.ini, exactly as with real servers.
#
# IPs and memory can be overridden from the environment, e.g.
#   NODE1_MEMORY=3072 vagrant up      # low-RAM host
#   K8S_NODES=master vagrant up       # bring up only one VM

NODES = {
  "master" => { ip: ENV.fetch("MASTER_IP", "192.168.56.10"), cpus: 2,
                memory: ENV.fetch("MASTER_MEMORY", "2048").to_i },
  "node1"  => { ip: ENV.fetch("NODE1_IP", "192.168.56.11"), cpus: 2,
                memory: ENV.fetch("NODE1_MEMORY", "4096").to_i },
}
selected = ENV.fetch("K8S_NODES", NODES.keys.join(",")).split(",").map(&:strip)

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_check_update = false
  config.vm.boot_timeout = 600                       # first boot can be slow
  config.vm.synced_folder ".", "/vagrant", disabled: true

  NODES.each do |name, spec|
    next unless selected.include?(name)

    config.vm.define name do |node|
      node.vm.hostname = name
      # enp0s3 = VirtualBox NAT (same 10.0.2.15 on every VM, used by `vagrant ssh`)
      # enp0s8 = this private network, the only one the VMs can reach each other on
      node.vm.network "private_network", ip: spec[:ip]

      node.vm.provider "virtualbox" do |vb|
        vb.name = "lab01-k8s-#{name}"
        vb.cpus = spec[:cpus]
        vb.memory = spec[:memory]
        vb.linked_clone = true
      end
    end
  end
end
