# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.ssh.private_key_path = ["~/.ssh/id_rsa","~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false
  
  config.vm.define "controller" do |controller|
    controller.vm.hostname = "controller"

    controller.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
    controller.vm.provision "shell", inline: <<-EOC
      sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
      sudo service ssh restart
    EOC

    controller.vm.provision "shell", inline: <<-EOC
      sudo apt-get install software-properties-common
      sudo apt-add-repository -y ppa:ansible/ansible
      sudo apt-get update -y
      sudo apt-get install ansible -y
    EOC

    controller.vm.network "private_network", ip:"192.168.100.10"
  end

  config.vm.define "ap" do |ap|
    ap.vm.box = "centos/7"

    ap.vm.hostname = "ap"
    ap.vm.network "forwarded_port", guest: "80", host: "8080", auto_correct: true
    ap.vm.network "private_network", ip: "192.168.100.11"
    ap.vm.provision "file", source: "./authorized_keys", destination: "~/.ssh/authorized_keys"
  
    ap.vm.provision "shell", inline: <<-EOC
      sudo yum update -y
      
    EOC

  end

end
