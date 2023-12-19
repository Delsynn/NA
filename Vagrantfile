Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.hostname = "ApplicationServer"
  config.vm.network "private_network", ip: "192.168.50.10"
  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.network "forwarded_port", guest: 4000, host: 4000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "4096"
    vb.cpus = 2
  end
  config.vm.provision "docker" do |d|
    d.pull_images "lissy93/dashy:latest"
    d.run "lissy93/dashy:latest", args: "-p 4000:4000"
    d.pull_images "amir20/dozzle:latest"
    d.run "amir20/dozzle:latest", args: "-p 8888:8080"
    d.pull_images "filegator/filegator"
    d.run "filegator/filegator", args: "-p 80:80"
  end
end