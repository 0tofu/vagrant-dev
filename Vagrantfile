# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.ssh.forward_agent = true

  config.vm.hostname = "vagrant-dev"
  config.vm.network "private_network", ip: "192.168.33.33"
  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.network "forwarded_port", guest: 443, host: 443
  config.vm.network "forwarded_port", guest: 4444, host: 4444
  config.vm.network "forwarded_port", guest: 5900, host: 5900

  config.disksize.size = "128GB"

  config.vm.provider "virtualbox" do |vm|
    vm.gui = false
    vm.customize ["modifyvm", :id, "--audio", "none"]
    vm.customize ["modifyvm", :id, "--cpus", "2", "--ioapic", "on"]
    vm.customize ["modifyvm", :id, "--memory", "8192"]
    # ubuntu/focal64の起動オプションとしてttyS0が有効となっていることが原因でpanicが起こるらしい対策
    vm.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
    vm.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
  end

  config.mutagen.orchestrate = true

  config.vm.synced_folder "~/.ssh", "/home/vagrant/.ssh",
    type: "rsync",
    rsync__auto: true,
    rsync__args: ["--verbose", "--archive", "-z", "--copy-links"],
    rsync__exclude: [".git/", ".DS_Store", "/**/.git/"]

  config.vm.synced_folder "./sites", "/home/vagrant/sites",
    type: "rsync",
    rsync__auto: true,
    rsync__args: ["--verbose", "--archive", "--delete", "-z"],
    rsync__exclude: [".idea/", ".git/", ".DS_Store", "/**/.idea/", "/**/.git/"]

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.verbose = true
    ansible.install_mode = "pip"
    ansible.pip_install_cmd = "sudo apt-get install -y python3-distutils && curl -s https://bootstrap.pypa.io/get-pip.py | sudo python3"
  end
end
