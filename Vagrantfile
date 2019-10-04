# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # Online Vagrantfile documentation is at https://docs.vagrantup.com.

  # The AWS provider does not actually need to use a Vagrant box file.
  config.vm.box = "dummy"

  config.vm.provider :aws do |aws, override|
    # We will gather the data for these three aws configuration
    # parameters from environment variables (more secure than
    # committing security credentials to your Vagrantfile).
    #
    # aws.access_key_id = "YOUR KEY"
    # aws.secret_access_key = "YOUR SECRET KEY"
    # aws.session_token = "SESSION TOKEN"

    # The region for Amazon Educate is fixed.
    aws.region = "us-east-1"

    # These options force synchronisation of files to the VM's
    # /vagrant directory using rsync, rather than using trying to use
    # SMB (which will not be available by default).
    override.nfs.functional = false
    override.vm.allowed_synced_folder_types = :rsync

    # Following the lab instructions should lead you to provide values
    # appropriate for your environment for the configuration variable
    # assignments preceded by double-hashes in the remainder of this
    # :aws configuration section.

    # The keypair_name parameter tells Amazon which public key to use.
    ##aws.keypair_name = ""
    # The private_key_path is a file location in your macOS account
    # (e.g., ~/.ssh/something).
    ##override.ssh.private_key_path = ""

    # Choose your Amazon EC2 instance type (t2.micro is cheap).
    ##aws.instance_type = "t2.micro"

    # You need to indicate the list of security groups your VM should
    # be in. Each security group will be of the form "sg-...", and
    # they should be comma-separated (if you use more than one) within
    # square brackets.
    #
    ##aws.security_groups = [""]

    # For Vagrant to deploy to EC2 for Amazon Educate accounts, it
    # seems that a specific availability_zone needs to be selected
    # (will be of the form "us-east-1a"). The subnet_id for that
    # availability_zone needs to be included, too (will be of the form
    # "subnet-...").
    ##aws.availability_zone = ""
    ##aws.subnet_id = ""

    # You need to chose the AMI (i.e., hard disk image) to use. This
    # will be of the form "ami-...".
    #
    # If you want to use Ubuntu Linux, you can discover the official
    # Ubuntu AMIs: https://cloud-images.ubuntu.com/locator/ec2/
    #
    # You need to get the region correct, and the correct form of
    # configuration (probably amd64, hvm:ebs-ssd, hvm).
    #
    ##aws.ami = ""

    # If using Ubuntu, you probably also need to uncomment the line
    # below, so that Vagrant connects using username "ubuntu".
    ##override.ssh.username = "ubuntu"
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end


# # -*- mode: ruby -*-
# # vi: set ft=ruby :
#
# # A Vagrantfile to set up three VMs, a webserver and a database server,
# # connected together using an internal network with manually-assigned
# # IP addresses for the VMs.
#
# Vagrant.configure("2") do |config|
#
#   config.vm.box = "ubuntu/xenial64"
#
#   config.vm.define "webserver" do |webserver|
#     webserver.vm.hostname = "webserver"
#     webserver.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
#
#     # The IP address and private network status allow for inter server
#     # communication and disallow external communication via this IP address.
#     webserver.vm.network "private_network", ip: "192.168.2.11"
#
#     # This is where you mark my assignment.
#     webserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=777"]
#
#     #This is where shell commands go that will provision this server.
#     webserver.vm.provision "shell", inline: <<-SHELL
#
#       # This keeps security sort-of up to date.
#       apt-get update
#
#       # This installs apache and php with necessary axillary paraphernalia.
#       apt-get install -y apache2 php libapache2-mod-php php-mysql
#
#       # Changing the VM's webserver's configuration to use a shared folder.
#       # (Look inside test-website.conf for specifics.)
#       cp /vagrant/test-website.conf /etc/apache2/sites-available/
#
#       # Installing the website's configuration and disabling the default.
#       a2ensite test-website
#       a2dissite 000-default
#       service apache2 reload
#
#     SHELL
#   end
#
#   config.vm.define "dbserver" do |dbserver|
#     dbserver.vm.hostname = "dbserver"
#
#     # Notice the slightly different number for an IP address.
#     dbserver.vm.network "private_network", ip: "192.168.2.12"
#
#     # This is where you mark my assignment.
#     dbserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=777"]
#
#     #This is where shell commands go that will provision this server.
#     dbserver.vm.provision "shell", inline: <<-SHELL
#
#       # This keeps security sort-of up to date.
#       apt-get update
#
#       # This is secret password that is required to HACK into the SQL database.
#       export MYSQL_PWD='insecure_mysqlroot_pw'
#
#       # This is the system HACKing itself.
#       echo "mysql-server mysql-server/root_password password $MYSQL_PWD" | debconf-set-selections
#       echo "mysql-server mysql-server/root_password_again password $MYSQL_PWD" | debconf-set-selections
#
#       # Now that we have HACKed the system we can get the database up and
#       # running.
#       apt-get -y install mysql-server
#
#       # This creates the database.
#       echo "CREATE DATABASE fvision;" | mysql
#
#       # This creates a database user "webuser" with the given password.
#       echo "CREATE USER 'webuser'@'%' IDENTIFIED BY 'insecure_db_pw';" | mysql
#
#       # This grants all permissions to the database user "webuser" regarding
#       # the "fvision" database that we just created, above.
#       echo "GRANT ALL PRIVILEGES ON fvision.* TO 'webuser'@'%'" | mysql
#
#       # This is straight up black magic.
#       export MYSQL_PWD='insecure_db_pw'
#
#       # This will let SQL run all the code in the setup-database.sql file.
#       cat /vagrant/setup-database.sql | mysql -u webuser fvision
#
#       # This allows the php to access the database. Changing SQL from accepting
#       # local connections to accepting connections from any network interface.
#       sed -i'' -e '/bind-address/s/127.0.0.1/0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
#
#       # We then restart the MySQL server to ensure that it picks up
#       # our configuration changes.
#       service mysql restart
#     SHELL
#   end
#
#   config.vm.define "dockerserver" do |dockerserver|
#     dockerserver.vm.hostname = "dockerserver"
#
#     # Yet again notice the slightly different number for an IP address.
#     dockerserver.vm.network "private_network", ip: "192.168.2.13"
#
#     # This is where you mark my assignment.
#     dockerserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=777"]
#
#     #This is where shell commands go that will provision this server.
#     dockerserver.vm.provision "shell", inline: <<-SHELL
#
#       # This keeps security sort-of up to date.
#       apt-get update
#
#       # Downloading Docker.
#       curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
#       sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
#       sudo apt-get update
#       apt-cache policy docker-ce
#       sudo apt-get install -y docker-ce
#
#       # This pulls and runs my container from Docker.
#       docker pull statuec/my-container
#       docker run statuec/my-container
#
#       # Removes all docker images on this VM, but leaves Docker running.
#       docker container prune
#     SHELL
#   end
# end
