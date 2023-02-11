# Vagrant IAAC
This project is a part of my personal learning phase for vagrant and IAAC(Infra as a code), i will deploy static and dynamique(wordpress) website using vagrantfile.

## Requirements
Install Vagrant
Install VirtualBox
## Usage(Static website)

- cd /website
- vagrant up (To start config)
- vagrant ssh (to login into the VM)
- open your browser and past this IP: 192.168.33.15
- vagrant halte (to stop the VM)
- vagrant destroy (to delete the VM)

# Vagrantfile for the static WebSite config

```

## Private network to access the VM
config.vm.network "private_network", ip: "192.168.33.15"

##(Optional) activate a bridge to your network
config.vm.network "public_network"

##provisioning 
 config.vm.provision "shell", inline: <<-SHELL
  	yum install httpd wget unzip -y
  	systemctl start httpd
  	systemctl enable httpd
  	cd /tmp/
  	wget https://www.tooplate.com/zip-templates/2127_little_fashion.zip
  	unzip 2127_little_fashion.zip
  	cp -r 2127_little_fashion/* /var/www/html
  	systemctl restart httpd
  SHELL
end