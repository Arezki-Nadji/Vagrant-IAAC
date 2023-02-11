# Vagrant IAAC
This project is a part of my personal learning phase for vagrant and IAAC(Infra as a code), i will deploy static and dynamique(wordpress) website using vagrantfile.

## Requirements
Install Vagrant
Install VirtualBox
## Usage(Static website) (Using CentOS 7)

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
```

## Usage(Wordpress) (Using CentOS 7)

- cd /website
- vagrant up (To start config)
- vagrant ssh (to login into the VM)
- open your browser and past this IP: 192.168.33.19
- vagrant halte (to stop the VM)
- vagrant destroy (to delete the VM)

# Vagrantfile for the static WebSite config
```

## Private network to access the VM
config.vm.network "private_network", ip: "192.168.33.19"
  config.vm.provision "shell", inline: <<-SHELL
  	sudo apt update
	sudo apt install apache2 \
		                 ghostscript \
		                 libapache2-mod-php \
		                 mysql-server \
		                 php \
		                 php-bcmath \
		                 php-curl \
		                 php-imagick \
		                 php-intl \
		                 php-json \
		                 php-mbstring \
		                 php-mysql \
		                 php-xml \
		                 php-zip -y

	sudo mkdir -p /srv/www
	sudo chown www-data: /srv/www
	curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
	sudo cp /vagrant/wordpress.conf /etc/apache2/sites-available/wordpress.conf

	sudo a2ensite wordpress
	sudo a2enmod rewrite
	sudo a2dissite 000-default
	sudo service apache2 reload

	sudo mysql -u root -e 'CREATE DATABASE wordpress;'
	sudo mysql -u root -e 'CREATE USER wordpress@localhost IDENTIFIED BY "admin123";'
	sudo mysql -u root -e 'GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;'
	sudo mysql -u root -e 'FLUSH PRIVILEGES;'

	sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php

	sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
	sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
	sudo -u www-data sed -i 's/password_here/admin123/' /srv/www/wordpress/wp-config.php


  SHELL
end
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
```