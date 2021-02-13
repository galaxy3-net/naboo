# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "cybersecurity/UbuntuVM"
  config.vm.hostname = "ucibox"
  config.vm.box_version = '1.2.0'
  config.vbguest.auto_update = false

  config.trigger.after :up do |trigger|
    trigger.name = "Complete Setup"
  	trigger.info = File.read("Description")
  end

  folders_vagrantfile = File.expand_path("#{ENV['G3HOME']}/vagrant/kamino/vagrant_includes/folders.txt", __FILE__)
  load folders_vagrantfile if File.exists?(folders_vagrantfile)

  ips_vagrantfile = File.expand_path("#{ENV['G3HOME']}/vagrant/kamino/vagrant_includes/ips.txt", __FILE__)
  load ips_vagrantfile if File.exists?(ips_vagrantfile)

  files_vagrantfile = File.expand_path("#{ENV['G3HOME']}/vagrant/kamino/vagrant_includes/files.txt", __FILE__)
  load files_vagrantfile if File.exists?(files_vagrantfile)



  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # Uncomment ONE the lines below to control how much RAM Vagrant gives the VM
    # We recommend starting with 4096 (4Gb), and moving down if necessary
    # vb.memory = "1024" # 1Gb
    # vb.memory = "2048" # 2Gb
    # vb.memory = "4096" # 4Gb
    vb.name = "Naboo (ucibox)"
    vb.gui = false
    vb.cpus = "4"
    vb.memory = "4096"
    vb.customize ["modifyvm", :id, "--description", File.read("Description")]
    vb.customize ['modifyvm', :id, '--vrde', 'off']
  end
   config.vm.provision "shell", inline: <<-SHELL
     apt-get install -y ansible python3
SHELL
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/vagrant/playbook.yml"
    ansible.galaxy_role_file = "/vagrant/requirements.yml"
    inventory_path = "/vagrant/hosts"
  end
end