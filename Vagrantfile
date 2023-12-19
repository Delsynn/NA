Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.hostname = "ApplicationServer"
  config.vm.network "private_network", ip: "192.168.50.10"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 5231, host: 5231
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "2048"
    vb.cpus = 2
  end
  config.vm.provision "docker" do |d|
    d.pull_images "filegator/filegator"
    d.run "filegator/filegator", args: "-p 8080:8080"
    d.pull_images "yourselfhosted/slash:latest"
    d.run "yourselfhosted/slash:latest", args: "-p 5231:5231"
  end
end