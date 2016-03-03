# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms = [
    [ "debian-jessie", "debian/jessie64", "192.168.65.90" ],
    [ "debian-stretch", "sharlak/debian_stretch_64", "192.168.65.91" ]
  ]

  vms_backuped = [
    [ "debian-backuped", "debian/jessie64", "192.168.65.92" ]
  ]

  config.vm.provider "virtualbox" do |v|
    v.cpus = 1
    v.memory = 256
  end

  vms.each do |vm|
    config.vm.define vm[0] do |m|
      m.vm.hostname = vm[0]
      m.vm.box = vm[1]
      m.vm.network "private_network", ip: vm[2]

      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.groups = { "test" => [ vm[0] ] }
        ansible.verbose = 'vv'
        ansible.sudo = true
      end
    end
  end

  vms_backuped.each do |vm|
    config.vm.define vm[0] do |m|
      m.vm.hostname = vm[0]
      m.vm.box = vm[1]
      m.vm.network "private_network", ip: vm[2]

      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/backuped.yml"
        ansible.groups = { "backuped" => [ vm[0] ] }
        ansible.verbose = 'vv'
        ansible.sudo = true
      end
    end
  end
end
