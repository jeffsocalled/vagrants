Vagrant.configure("2") do |config|
  config.vm.define "web" do | web |
    web.vm.box = "ubuntu/jammy64"
    #web.vm.box_version = "7.0"
    web.vm.network "private_network", ip: "192.168.56.10"
    web.vm.network "public_network"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.hostname="webvm"

    web.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
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