Vagrant.configure("2") do |config|
  config.vm.define "web" do | web |
    web.vm.box = "ubuntu/jammy64"
    #web.vm.box_version = "7.0"
    web.vm.network "private_network", ip: "192.168.56.10"
    web.vm.network "public_network"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.hostname="webvm"

    web.vm.provision "shell", inline: <<-SHELL
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
     
     cat > /etc/apache2/sites-available/wordpress.conf <<EOF
     <VirtualHost *:80>
         DocumentRoot /srv/www/wordpress
         <Directory /srv/www/wordpress>
             Options FollowSymLinks
             AllowOverride Limit Options FileInfo
             DirectoryIndex index.php
             Require all granted
         </Directory>
         <Directory /srv/www/wordpress/wp-content>
             Options FollowSymLinks
             Require all granted
         </Directory>
      </VirtualHost>
EOF
     
     sudo a2ensite wordpress
     sudo a2enmod rewrite
     sudo a2dissite 000-default
     
     
     mysql -u root -e 'CREATE DATABASE wordpress;'
     mysql -u root -e 'CREATE USER wordpress@localhost IDENTIFIED BY "admin123";'
     mysql -u root -e 'GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;'   
     mysql -u root -e 'FLUSH PRIVILEGES;'
     
     sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
     sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
     sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
     sudo -u www-data sed -i 's/password_here/admin123/' /srv/www/wordpress/wp-config.php
     
     systemctl restart mysql
     systemctl restart apache2

    SHELL
  end

  config.vm.define "db" do | db |
    db.vm.box = "ubuntu/jammy64"
    #db.vm.box_version = "7.0"
    db.vm.network "private_network", ip: "192.168.56.11"
    db.vm.network "public_network"
    db.vm.network "forwarded_port", guest: 80, host: 9090
    db.vm.hostname="dbvm"
  end
 
end