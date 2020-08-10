# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yaml")
g3home		   = ENV['G3HOME']
g3_config	   = YAML.load_file("#{g3home}/g3.yaml")
g3branch         = ENV['G3BRANCH']
vagrant_config = configs['configs'][g3branch]

thedr_userid = g3_config['g3'][g3branch]['userid']
thedr_groupid = g3_config['g3'][g3branch]['groupid']

Vagrant.configure("2") do |config|
  config.vm.box = "cybersecurity/UbuntuVM"
  config.vm.hostname = "naboo"

  #config.vm.network "private_network", ip: "10.55.55.9"

  config.vm.synced_folder	"../../",	        "/vagrant",   owner: thedr_userid, group: thedr_groupid
  config.vm.synced_folder "../../repos",        "/repos",     owner: thedr_userid, group: thedr_groupid, create: true
  config.vm.synced_folder "../../Downloads",    "/Downloads", owner: thedr_userid, group: thedr_groupid, create: true
  #config.vm.synced_folder "../../log/nakadia", "/var/log/",  owner: thedr_userid, group: thedr_groupid, create: true

  config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 8080, host: 8080, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 3389, host: 3389, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 5901, host: 5901, host_ip: "127.0.0.1", auto_correct: true

  config.vm.network "private_network", ip: vagrant_config['private_ip']

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # Uncomment ONE the lines below to control how much RAM Vagrant gives the VM
    # We recommend starting with 4096 (4Gb), and moving down if necessary
    # vb.memory = "1024" # 1Gb
    # vb.memory = "2048" # 2Gb
    # vb.memory = "4096" # 4Gb
    vb.name = "Naboo (UCIBox)"
    # vb.gui = false
    vb.cpus = vagrant_config['virtualbox']['cpus']
    vb.memory = vagrant_config['virtualbox']['memory']
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]

    vb.customize ['modifyvm', :id, '--vrde', 'on']
    vb.customize ['modifyvm', :id, '--vrdeport', '5002']
    vb.customize ['modifyvm', :id, '--graphicscontroller', 'vboxsvga']
    vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
    #vb.customize ['modifyvm', :id, '--firmware', 'efi64']
    #vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
    vb.customize ['modifyvm', :id, '--description', vagrant_config['description']]
  end
   config.vm.provision "shell", inline: <<-SHELL
     tr -d '\r' < /vagrant/functions/ready >/usr/local/bin/ready && chmod 0700 /usr/local/bin/ready
     /usr/local/bin/ready
     /usr/local/bin/install_pkgs "supervisor" | tee -a /var/log/install_pkgs.log 2>&1
     #/usr/local/bin/pull_repos

     iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
     iptables -A INPUT -p tcp --dport 3389 -m state --state NEW -j ACCEPT

     setup_xrdp
     #setup_vnc
SHELL
end