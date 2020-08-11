# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.ssh.forward_agent = true

  config.vm.hostname = "vagrant-dev"
  config.vm.network "private_network", ip: "192.168.33.33"
   config.vm.network "forwarded_port", guest: 80, host: 80, host_ip: "127.0.0.1"

  config.disksize.size = "64GB"
  config.mutagen.orchestrate = true

  config.vm.provider "virtualbox" do |vm|
    vm.gui = false
    vm.customize ["modifyvm", :id, "--cpus", "2", "--ioapic", "on"]
    vm.customize ["modifyvm", :id, "--memory", "4096"]
    # ubuntu/focal64の起動オプションとしてttyS0が有効となっていることが原因でpanicが起こるらしい対策
    vm.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
    vm.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
  end

  config.vm.synced_folder "./sites", "/sites", type: "rsync",
    rsync_auto: true,
    rsync__exclude: [".git/", ".idea/", "node_modules/"]

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.verbose = true
    ansible.install_mode = "pip"
  end
end
