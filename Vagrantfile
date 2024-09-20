Vagrant.configure("2") do |config|
  config.vm.define "web" do | web |
    web.vm.box = "ubuntu/jammy64"
    #web.vm.box_version = "7.0"
    web.vm.network "private_network", ip: "192.168.56.10"
    web.vm.network "public_network"
    web.vm.network "forwarded_port", guest: 8080, host: 8080
    web.vm.hostname="webvm"

    web.vm.provision "shell", inline: <<-SHELL  
    # Install Apache Tomcat
    sudo apt update
    sudo apt install -y default-jdk
    wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.95/bin/apache-tomcat-9.0.95.tar.gz
    tar -xvf apache-tomcat-9.0.95.tar.gz
    sudo mv apache-tomcat-9.0.95 /opt/tomcat
    sudo chmod -R 755 /opt/tomcat

    # Create systemd service file
    sudo tee /etc/systemd/system/tomcat.service <<EOF
[Unit]
Description=Apache Tomcat Web Application Container
Wants=network.target
After=network.target

[Service]
Type=forking

Environment=CATALINA_BASE=/opt/tomcat
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_TMPDIR=/opt/tomcat/temp
Environment=JRE_HOME=/usr
Environment=CLASSPATH=/opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh


[Install]
WantedBy=multi-user.target
EOF

    # Create tomcat user and group
    sudo groupadd tomcat
    sudo useradd --home-dir /opt/tomcat --shell /sbin/nologin tomcat

    # Reload systemd daemon
    sudo systemctl daemon-reload

    # Start and enable Tomcat service
    sudo systemctl start tomcat
    sudo systemctl enable tomcat
  SHELL

  end

#  config.vm.define "db" do | db |
#    db.vm.box = "ubuntu/jammy64"
#    #db.vm.box_version = "7.0"
#    db.vm.network "private_network", ip: "192.168.56.11"
#    db.vm.network "public_network"
#    db.vm.network "forwarded_port", guest: 80, host: 9090
#    db.vm.hostname="dbvm"
#  end
 
end