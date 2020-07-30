# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "cybersecurity/UbuntuVM"
  config.vm.hostname = "ucibox"

  #config.vm.network "private_network", ip: "10.55.55.9"

  config.vm.synced_folder	"../../",	"/vagrant", owner: "2001", group: "2001"
  config.vm.synced_folder "../../repos", "/repos", owner: "2001", group: "2001", create: true
  config.vm.synced_folder "../../Downloads", "/Downloads", owner: "2001", group: "2001", create: true
  #config.vm.synced_folder "../../log/nakadia", "/var/log/", owner: "2001", group: "2001", create: true

  config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 3389, host: 3389, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 5901, host: 5901, host_ip: "127.0.0.1", auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # Uncomment ONE the lines below to control how much RAM Vagrant gives the VM
    # We recommend starting with 4096 (4Gb), and moving down if necessary
    # vb.memory = "1024" # 1Gb
    # vb.memory = "2048" # 2Gb
    # vb.memory = "4096" # 4Gb
    vb.name = "UCIBox"
    # vb.gui = false
    vb.cpus = "4"
    vb.memory = "4096"
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]

    vb.customize ['modifyvm', :id, '--vrde', 'on']
    vb.customize ['modifyvm', :id, '--vrdeport', '5002']
    vb.customize ['modifyvm', :id, '--graphicscontroller', 'vboxsvga']
    #vb.customize ['modifyvm', :id, '--firmware', 'efi64']
    #vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
  end
   config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get autoremove -y
     apt-get install -yq net-tools ansible dos2unix supervisor htop
     apt-get clean
     apt-get autoclean

     file /vagrant/functions/ready | grep CRLF && dos2unix -n /vagrant/functions/ready /usr/local/bin/ready
     file /vagrant/functions/ready | grep CRLF || cp /vagrant/functions/ready /usr/local/bin/ready
     chmod 0700 /usr/local/bin/ready
     ready

     pull_repos

     #iptables -F
     #iptables -X
     #iptables -P INPUT ACCEPT
     #iptables -P OUTPUT ACCEPT
     #iptables -P FORWARD ACCEPT
     iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
     iptables -A INPUT -p tcp --dport 3389 -m state --state NEW -j ACCEPT

     setup_xrdp
     setup_vnc
SHELL
end