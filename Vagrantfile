# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  Environments = Array["development", "staging", "production"]
  ServersCount = 3

  (1..ServersCount).each do |i|
    config.vm.define "sandbox-#{Environments.at(i-1)}" do |sandbox1|
      sandbox1.vm.box = "ubuntu/bionic64"
      sandbox1.vm.hostname = Environments.at(i-1)
      sandbox1.vm.network "forwarded_port", guest: 80, host: "806#{i}", id: "Apache Web Server"
      sandbox1.vm.network "private_network", ip: "172.42.42.10#{i}"
      sandbox1.vm.synced_folder "../web-server-data-#{i}", "/var/www/html"
      # This is a completly new comment
      # This is a comment
    
      sandbox1.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.memory = 1024
        vb.cpus = 1
        vb.name = "sandbox-#{Environments.at(i-1)}"
      end

      sandbox1.vm.provision "docker"
      sandbox1.vm.provision "shell", inline: <<-SHELL
        chmod +x /vagrant/sandbox1-provisioner.sh
        /vagrant/sandbox1-provisioner.sh
      SHELL
    end
  end

  config.vm.define "sandbox-podman" do |sandbox2|
    sandbox2.vm.box = "ubuntu/bionic64"
    sandbox2.vm.hostname = "podman"
    sandbox2.vm.network "forwarded_port", guest: 80, host: 8070, id: "NGINX Web Server"
    sandbox2.vm.network "private_network", ip: "172.42.42.110"
    sandbox2.vm.synced_folder "../nginx-data", "/var/www/html"
    
    sandbox2.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = 1024
      vb.cpus = 1
      vb.name = "sandbox-podman"
    end
  
    sandbox2.vm.provision "shell", inline: <<-SHELL
      chmod +x /vagrant/sandbox2-provisioner.sh
      /vagrant/sandbox2-provisioner.sh
    SHELL
  end
end
