# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.box = "dummy"

  config.vm.provider :aws do |aws, override|

    aws.region = "us-east-1"

    override.nfs.functional = false
    override.vm.allowed_synced_folder_types = :rsync

    aws.keypair_name = "cosc349"

    override.ssh.private_key_path = "~/.ssh/cosc349.pem"

    aws.instance_type = "t2.micro"

    aws.security_groups = ["sg-0aed94ee69196de29"]

    aws.availability_zone = "us-east-1b"
    aws.subnet_id = "subnet-e57b98a8"

    aws.ami = "ami-04763b3055de4860b"

    override.ssh.username = "ubuntu"
  end

  config.vm.define "webserver" do |webserver|
    
  end

  config.vm.define "dbserver" do |dbserver|

  end

  config.vm.define "dockerserver" do |dockerserver|

  end
end
