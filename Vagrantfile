# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	config.vm.provider "virtualbox" do |vb|

		# Set up serial port
		# name = /dev/ttyS0
		# IO address = 0x3F8
		# Interupt Request (IRQ) = 4
		vb.customize ["modifyvm", :id, "--uart1", "0x3f8", "4"]
	  end
  
  config.vm.define "spproto" do |spproto|
  
    spproto.vm.box = "ubuntu/bionic64"
    spproto.vm.network "private_network", ip: "192.168.33.110"
	spproto.vm.network "forwarded_port", guest: 6379, host: 6379, host_ip: "127.0.0.1"
	
	# Disable the default vagrant shared folder and sync to standard GOPATH.
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.synced_folder ".", "/home/vagrant/spproto/"
	
	spproto.vm.provision "shell", inline: <<-SHELL
	  echo "Register Microsoft key and feed"
	  wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
	  sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
	  wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list
	  sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
	  sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
	  sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
	  
	  echo "Install .NET SDK"
	  asud apt-get install snap
	  sudo apt-get install -y apt-transport-https
	  sudo apt-get update
	  sudo snap install dotnet-sdk --beta --classic
	  sudo snap alias dotnet-sdk.dotnet dotnet
	  echo "export PATH=$PATH:/snap/bin" >> ~/.bashrc
	  source ~/.bashrc


	  
	  echo "Install remote debugger"
	  # sudo apt-get install -y openssh-server unzip curl
	  # curl -sSL https://aka.ms/getvsdbgsh | bash /dev/stdin -v latest -l /home/vagrant/vsdbg
	  
	  # echo "Install Redis"
	  # sudo apt-get install -y redis-server
	  # echo "configure redis to be accessible from outside"
	  # sudo sed -i 's/bind 127.0.0.1/bind 0.0.0.0/g' /etc/redis/redis.conf
	  # sudo service redis-server restart
	  
    SHELL
    
  end
  
  # Fazi Database
  # config.vm.define "database" do |db|
    # db.vm.box = "ubuntu/bionic64"
    # db.vm.network "private_network", ip: "192.168.33.111"
	
	# config.vm.synced_folder "Database", "/Database"
	
	# db.vm.provision "shell", inline: <<-SHELL
			# #TODO: add MS SQL install commands
		# SHELL
  # end
  
end
