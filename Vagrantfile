# -*- mode: ruby -*-
# vi: set ft=ruby :

# A Vagrantfile to set up three VMs, a webserver and a database server,
# connected together using an internal network with manually-assigned
# IP addresses for the VMs.

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  
  config.vm.define "webserver" do |webserver|
    webserver.vm.hostname = "webserver"
    webserver.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

    # The IP address and private network status allow for inter server
    # communication and disallow external communication via this IP address.
    webserver.vm.network "private_network", ip: "192.168.2.11"

    # This is where you mark my assignment.
    webserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=777"]

    #This is where shell commands go that will provision this server.
    webserver.vm.provision "shell", inline: <<-SHELL

      # This keeps security sort-of up to date.
      apt-get update

      # This installs apache and php with necessary axillary paraphernalia.
      apt-get install -y apache2 php libapache2-mod-php php-mysql

      # Changing the VM's webserver's configuration to use a shared folder.
      # (Look inside test-website.conf for specifics.)
      cp /vagrant/test-website.conf /etc/apache2/sites-available/

      # Installing the website's configuration and disabling the default.
      a2ensite test-website
      a2dissite 000-default
      service apache2 reload

    SHELL
  end

  config.vm.define "dbserver" do |dbserver|
    dbserver.vm.hostname = "dbserver"

    # Notice the slightly different number for an IP address.
    dbserver.vm.network "private_network", ip: "192.168.2.12"

    # This is where you mark my assignment.
    dbserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=777"]

    #This is where shell commands go that will provision this server.
    dbserver.vm.provision "shell", inline: <<-SHELL

      # This keeps security sort-of up to date.
      apt-get update

      # This is secret password that is required to HACK into the SQL database.
      export MYSQL_PWD='insecure_mysqlroot_pw'

      # This is the system HACKing itself.
      echo "mysql-server mysql-server/root_password password $MYSQL_PWD" | debconf-set-selections
      echo "mysql-server mysql-server/root_password_again password $MYSQL_PWD" | debconf-set-selections

      # Now that we have HACKed the system we can get the database up and
      # running.
      apt-get -y install mysql-server

      # This creates the database.
      echo "CREATE DATABASE fvision;" | mysql

      # This creates a database user "webuser" with the given password.
      echo "CREATE USER 'webuser'@'%' IDENTIFIED BY 'insecure_db_pw';" | mysql

      # This grants all permissions to the database user "webuser" regarding
      # the "fvision" database that we just created, above.
      echo "GRANT ALL PRIVILEGES ON fvision.* TO 'webuser'@'%'" | mysql

      # This is straight up black magic.
      export MYSQL_PWD='insecure_db_pw'

      # This will let SQL run all the code in the setup-database.sql file.
      cat /vagrant/setup-database.sql | mysql -u webuser fvision

      # This allows the php to access the database. Changing SQL from accepting
      # local connections to accepting connections from any network interface.
      sed -i'' -e '/bind-address/s/127.0.0.1/0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf

      # We then restart the MySQL server to ensure that it picks up
      # our configuration changes.
      service mysql restart
    SHELL
  end
  
  config.vm.define "dockerserver" do |dockerserver|
    dockerserver.vm.hostname = "dockerserver"

    # Yet again notice the slightly different number for an IP address.
    dockerserver.vm.network "private_network", ip: "192.168.2.13"

    # This is where you mark my assignment.
    dockerserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=777"]

    #This is where shell commands go that will provision this server.
    dockerserver.vm.provision "shell", inline: <<-SHELL

      # This keeps security sort-of up to date.
      apt-get update

      # Downloading Docker.
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
      sudo apt-get update
      apt-cache policy docker-ce
      sudo apt-get install -y docker-ce

      # This pulls and runs my container from Docker.
      docker pull statuec/my-container
      docker run statuec/my-container

      # Removes all docker images on this VM, but leaves Docker running.
      docker container prune
    SHELL
  end  
end
