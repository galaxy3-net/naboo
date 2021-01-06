# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "cybersecurity/UbuntuVM"
  config.vm.hostname = "ucibox"
  config.vm.box_version = '1.2.0'

  config.vbguest.auto_update = false

  config.vm.network "private_network", ip: "10.55.55.9",
  	virtualbox__intnet: "g3main"

  config.trigger.after :up do |trigger|
    trigger.name = "Complete Setup"
  	trigger.info = File.read("Description")
  end
  config.vm.synced_folder	"../../",	"/vagrant", owner: "1001", group: "1001"
  config.vm.synced_folder "~/repos/uci", "/repos", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true
  config.vm.synced_folder "../../Downloads", "/Downloads", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true
  #config.vm.synced_folder "../../log/nakadia", "/var/log/", owner: "1001", group: "1001", mount_options: ["fmode=777", "dmode=777"], create: true

  config.vm.network "forwarded_port", guest: 22, host: 2200, id: "ssh", disabled: true
  config.vm.network "forwarded_port", guest: 22, host: 29022, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 29000, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 80, host: 29080, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 3389, host: 29389, host_ip: "0.0.0.0", auto_correct: true
  config.vm.network "forwarded_port", guest: 5901, host: 29901, host_ip: "127.0.0.1", auto_correct: true

  config.vm.provision "file", source: "playbook.yml", destination: "playbook.yml"
  config.vm.provision "file", source: "../../functions", destination: "functions/bin"
  config.vm.provision "file", source: "hosts", destination: "hosts"
  config.vm.provision "file", source: "requirements.yml", destination: "requirements.yml"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # Uncomment ONE the lines below to control how much RAM Vagrant gives the VM
    # We recommend starting with 4096 (4Gb), and moving down if necessary
    # vb.memory = "1024" # 1Gb
    # vb.memory = "2048" # 2Gb
    # vb.memory = "4096" # 4Gb
    vb.name = ENV['boxname']
    vb.gui = false
    vb.cpus = "4"
    vb.memory = "4096"
    vb.customize ["modifyvm", :id, "--description", File.read("Description")]
#    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]

    vb.customize ['modifyvm', :id, '--vrde', 'off']
    #vb.customize ['modifyvm', :id, '--vrdeaddress', '0.0.0.0']
    #vb.customize ['modifyvm', :id, '--vrdeport', '2200']
    #vb.customize ['modifyvm', :id, '--graphicscontroller', 'vboxsvga']
    #vb.customize ['modifyvm', :id, '--firmware', 'efi64']
    #vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
  end
   config.vm.provision "shell", inline: <<-SHELL
     apt-get install -y ansible python3
SHELL
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/vagrant/vagrant/naboo/playbook.yml"
    ansible.galaxy_role_file = "/home/vagrant/requirements.yml"
    inventory_path = "/home/vagrant/hosts"
  end
end