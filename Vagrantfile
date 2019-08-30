# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2 php libapache2-mod-php

    # Changing the VM's webserver's configuration to use a shared folder.
    # (Look inside test-website.conf for specifics.)
    cp /vagrant/test-website.conf /etc/apache2/sites-available/
    # Installing our website's configuration and disabling the default.
    a2ensite test-website
    a2dissite 000-default
    service apache2 reload
  SHELL
end
