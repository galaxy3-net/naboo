# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "cybersecurity/UbuntuVM"
  config.vm.hostname = "ucibox"
  config.vm.box_version = '1.2.6'
  config.vbguest.auto_update = false

  config.trigger.after :up do |trigger|
    trigger.name = "Complete Setup"
  	trigger.info = File.read("Description")
  end

  config.vm.synced_folder	"./",	"/vagrant", owner: "1001", group: "1001"
  config.vm.synced_folder "~/repos/uci", "/repos", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true
  config.vm.synced_folder "../../Downloads", "/Downloads", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true
  config.vm.synced_folder "../../armory", "/Armory", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true
  #config.vm.synced_folder "../../log/nakadia", "/var/log/", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true

  #folders_vagrantfile = File.expand_path("#{ENV['G3HOME']}/vagrant/kamino/vagrant_includes/folders.txt", __FILE__)
  #load folders_vagrantfile if File.exists?(folders_vagrantfile)

  config.vm.network "forwarded_port", guest: 22, host: 2200, id: "ssh", disabled: true
  config.vm.network "forwarded_port", guest: 22, host: 29022, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 221, host: 221, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 25, host: 25, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 80, host: 29080, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 8081, host: 8081, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 8082, host: 8082, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 8083, host: 8083, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 3389, host: 29389, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5901, host: 29901, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5902, host: 29902, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5903, host: 29903, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5904, host: 29904, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5800, host: 29800, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 8080, host: 29880, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5900, host: 29900, host_ip: "0.0.0.0", auto_correct: true

  config.vm.network "private_network", ip: "10.55.55.9", virtualbox__intnet: "g3main"
    config.vm.network "private_network", ip: "10.55.56.9",
    	virtualbox__intnet: "metasploitable3",
    	mac: "080027aaaaaa"

  #ips_vagrantfile = File.expand_path("#{ENV['G3HOME']}/vagrant/kamino/vagrant_includes/ips.txt", __FILE__)
  #load ips_vagrantfile if File.exists?(ips_vagrantfile)

  #  config.vm.provision "file", source: "playbook.yml", destination: "playbook.yml"
  config.vm.provision "file", source: "../../functions", destination: "functions/bin"
  #  config.vm.provision "file", source: "hosts", destination: "hosts"
  #  config.vm.provision "file", source: "requirements.yml", destination: "requirements.yml"

  #files_vagrantfile = File.expand_path("#{ENV['G3HOME']}/vagrant/kamino/vagrant_includes/files.txt", __FILE__)
  #load files_vagrantfile if File.exists?(files_vagrantfile)



  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # Uncomment ONE the lines below to control how much RAM Vagrant gives the VM
    # We recommend starting with 4096 (4Gb), and moving down if necessary
    # vb.memory = "1024" # 1Gb
    # vb.memory = "2048" # 2Gb
    # vb.memory = "4096" # 4Gb
    vb.name = "Naboo (ucibox)"
    vb.gui = false
    vb.cpus = "2"
    vb.memory = "2048"
    vb.customize ["modifyvm", :id, "--description", File.read("Description")]
    vb.customize ['modifyvm', :id, '--vrde', 'off']
    vb.customize ['modifyvm', :id, '--vram', '128']
    vb.customize ['modifyvm', :id, '--graphicscontroller', 'vboxsvga']
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

#  Requirements.yml comes from ./armory/vagrant/setup/files/etc/ansible/requirements.yml