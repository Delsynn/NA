Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.hostname = "ApplicationServer"
  config.vm.network "public_network"
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 8888, host: 8888
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 8443, host: 8443
  config.vm.network "forwarded_port", guest: 3030, host: 3030
  config.vm.network "forwarded_port", guest: 5001, host: 5001
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
    vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
    vb.gui = true
    vb.memory = "8192"
    vb.cpus = 3
  end
  config.vm.provision "docker" do |d|
    d.pull_images "amir20/dozzle:latest"
    d.run "amir20/dozzle:latest", args: "-d --volume=/var/run/docker.sock:/var/run/docker.sock -p 8888:8080"
    d.pull_images "filegator/filegator"
    d.run "filegator/filegator", args: "-p 5000:8080"
    d.pull_images "delsynn/reactle:latest"
    d.run "delsynn/reactle:latest", args: "-d -p 3000:3000"
    d.pull_images "jgraph/drawio:latest"
    d.run "jgraph/drawio:latest", args: "-it -p 8080:8080 -p 8443:8443"
    d.pull_images "ekzhang/rustpad:latest"
    d.run "ekzhang/rustpad:latest", args: "-d -p 3030:3030"
    d.pull_images "lovasoa/wbo:latest"
    d.run "lovasoa/wbo:latest", args: "-it -d -p 5001:80"
  end
end